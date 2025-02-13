---
title: AWS S3를 사용한 이미지 업로드
slug: s3-image-upload
categories:
  - web
---

## 막연했던 파일 업로드 하기
"우리 동네 스터디" 프로젝트를 진행하면서 회원 profile에 이미지를 업로드하는 기능을 넣어보기로 했다. 
기획 단계에서 위치 정보를 어떻게 구현할지 고민했을 때와 마찬가지로 "이미지 그거 뭐 Multiparts?" 쓰면 된다는데요? 하면서 간단하게 결정했었다.

당연하게도 한 번도 사용해보지 않았었고 막상 구현을 앞두니 Postman으로 글자는 보내봤는데 
파일은 어떻게 보내지...? 하는 생각이 계속 들었다. 

다행스럽게도 MultiParts는 은근히 자료가 많아서 찾아서 구현하는데 큰 어려움은 없었지만 어떤 원리로 이루어지는지 
완벽히 이해하고 넘어갈 시간이 없었기에 일단 HTTP 요청때 데이터를 쪼개서 보낸다고 이해하고 실제로 적용 하는것에 집중했다. 

---


## 이미지 저장 위치

클라이언트 측에서 이미지를 받아와야 하니 다음과 같이 controller 에서 MultipartFile을  
@RequestBody로 받아온다.

이번에 새로 알게 된 점은, MultipartFile을 사용하기 위해서는 MultipartResolver 인터페이스를 Bean으로 등록해야 한다. 
하지만 Spring Boot에서는 해당 과정을 알아서 처리했기 때문에 나는 저 사실을 모르고 계속 진행했던 것...


그렇게 받아온 이미지를 어떤 식으로 저장해서 사용해야 할지가 2번째 고민이었다. 
처음에는 그냥 DB에 저장하면 되지 않을까 했다. 하지만 현재 프로젝트의 규모에선 별문제가 없더라도 
실제로 대규모 프로젝트나 서비스에서는 자원 관리와 비용 측면에서 매우 비효율적일 것 같다고 생각했다. 

그래서 생각한 점이 실제 파일은 S3 버킷에 저장하고 DB에는 해당 버킷에 올라간 이미지의 URL만 저장하는 것이었다. 
회원 entity에 image 테이블을 만들고, 거기에 URL이 담겨있으니 클라이언트 측에서는 그냥 해당 URL만 조회하면 된다. 

---

## AWS S3와 연동하기

이제 코드상으로 S3을 사용하기 위해 S3를 생성하고 만들어논 IAM user에 S3권한을 추가해주었다.  
그리고 아래와 같이 별도로 관리하던 민감 설정 정보 파일에 정보를 추가했다.
```yaml
#AWS Configurations
cloud:
  aws:
    s3:
      bucket: mainproject-bucket
    credentials:
      access-key: {IAM access}
      secret-key: {IAM secret}
    region:
      static: ap-northeast-2
    stack:
      auto: false
```

이전 프리 프로젝트때 이런 secret들이 깃허브에 당당하게 올라갔던 참사가 있어서 이번에는 이런식으로 분리를 해놓고 gitignore에 등록 해놓고 사용했다.

Controller에서 요청을 받으면 AwsS3Upload클래스로 들어오고 해당 부분에서 이미지 관련 작업들을 구현했다. 
S3와 연동해서 사용할 것이기 때문에 AWS SDK를 gradle에 추가하고, AmazonS3객체를 사용했다. 
```java
@Value("${cloud.aws.s3.bucket}")
    private String bucket; //위의 민감정보 설정 파일 환경변수로 사용

    private final AmazonS3 amazonS3; //AWS SDK 
    //private final MemberRepository memberRepository;
```
사용자가 이미 등록된 사진을 교체할 경우를 위해 member entity의 image컬럼을 업데이트를 하려고 
MemberRepository도 같이 선언했었으나 AwsS3Upload클래스는 업로드를 위한 클래스이기 때문에  
MemberService쪽으로 빼서 url을 수정하도록 변경했었다.


-----


### Upload 메서드

```java
public String upload(MultipartFile multipartFile, Member member) throws IOException {

        //이미 이미지가 등록되있으면 그거 삭제후 등록
        //카카오는 처음에는 그냥 진행하고 url 덮어쓰기 (s3에는 처음에 안들어가있음) 
        if (member.getProfileImageUrl() != null) {
            if (!member.getProfileImageUrl().contains("kakaocdn")) {
                delete(member);
            }
        }
        //파일이름 중복 방지 {UUID랜덤값 + 파일 원래 이름}
        String s3FileName = UUID.randomUUID() + "-" + multipartFile.getOriginalFilename();
        
        //파일 사이즈를 S3에 전달 (ObjectMetadata)
        ObjectMetadata objMeta = new ObjectMetadata();
        objMeta.setContentLength(multipartFile.getInputStream().available());

        //실제 이미지 업로드 하는 부분
        amazonS3.putObject(bucket, s3FileName, multipartFile.getInputStream(), objMeta);
        log.debug("Successfully uploaded:{} ", s3FileName);

        //업로드 된 이미지의 url 정보 String으로 반환
        return amazonS3.getUrl(bucket, s3FileName).toString();
    }
```
실질적인 업로드는 AmazonS3객체의 putObject메서드를 통해 이루어진다. 그리고 업로드가 이루어지면 해당 이미지의 
url정보를 String으로 받아와 리턴하고, 아래처럼 controller단에서 -> 서비스단 -> 멤버 entity에 반영한다. 

```java
Member imageUpdated = memberService.uploadImage(member, savedImagePath);
``` 

<br>
추가적으로, 우리는 OAuth를 통한 카카오 로그인을 구현했기 때문에 소셜 로그인을 통해 가입하게 된다면 
보통 프로필 이미지가 이미 등록이 되어있다. 

이때는 _http://k.kakaocdn.net/dn/VtKYM/bt....._식으로 url이 저장이 되어있는데 "kakaocdn"부분이 
계속 들어가는것을 확인하고 url에 저 부분이 포함되어 있는지의 여부로 분기를 걸었다. 

카카오 로그인을 했는데 카카오에서 넘어온 프로필 사진이 아닌 새 사진으로 업로드를 하고 싶다면 
처음에는 그냥 일반 upload로 진행하게 된다. 이미지 사진은 있지만 S3상에는 저장이 안되어 있어서  
처음엔 delete를 거칠 필요가 없다. 


-----


### delete 메서드



```java
    private void delete(Member member) {
        
        String s = member.getProfileImageUrl();
        AmazonS3URI s3URI = new AmazonS3URI(s);
        String key = URLDecoder.decode((s3URI.getKey().toString()), StandardCharsets.UTF_8);
        try {
            DeleteObjectRequest deleteRequest = new DeleteObjectRequest(bucket, key);
            amazonS3.deleteObject(deleteRequest);
            log.debug("Deleted previous image:{} ", key);
        } catch (Exception e) {
            e.printStackTrace();
            log.error("Exception ERROR: {} ", e.getMessage());
            throw e;
        }
    }
```
업로드를 구현했는데 이미지를 새로 바꿀때 마다 S3에 이전 이미지들이 남아돌기 시작했다. 
보기에도 좋지 않고 장기적으로는 성능상의 문제가 분명히 생길것이 뻔했기 때문에 회원이 이미지를 교체한다면 
현재 프로필 이미지를 s3에서 삭제를 하고, 새 이미지를 올리는 형식으로 구현하도록 했다. 

**S3에서 삭제하기 위해서는 S3상의 이미지 URI가 필요하고, DeleteObjectRequest객체를 사용**해야한다. 
DeleteObjectRequest는 String타입의 bucketName과 key를 파라미터로 받는다. 
bucketName은 말 그대로 버킷의 이름이고 **key는 해당 오브젝트의 주소**이다. 

```java
DeleteObjectRequest deleteRequest = new DeleteObjectRequest(bucket, key);
amazonS3.deleteObject(deleteRequest);
``` 
<br>
그리고 최종적으로 **AmazonS3 인터페이스의 deleteObject를 사용하고 DeleteObjectRequest를 파라미터**로 넣는다.

여기서 살짝 삽질을 했었는데, 업로드 파일이 한글 이름일 경우 %%2%%4%%이런식으로 깨지게 되어 
key값을 넣어도 삭제가 되지 않았다. 따라서 key값을 뽑는 시점에 UTF-8로 디코딩 하는 부분을 추가해서 넣었다. 

```java
String key = URLDecoder.decode((s3URI.getKey().toString()), StandardCharsets.UTF_8);
```
<br>
---


## 삭제가 또 안된다 

삭제까지 전부 구현을 했는데 삭제 요청을 보낼때마다 403에러가 떴다. 
아무리 찾아봐도 틀린점이 없었고 버킷의 Policy, IAM권한 확인 등 공식문서의 troubleshooting까지 확인했지만 
해결이 되지 않았다. 

디버그를 찍어보았는데 delete 요청 시점에 403이 떴고, 403은 권한 관련 에러라고 나와 처음부터 다시 해본다는
느낌으로 IAM유저를 새로 만들고 적용을 했는데 그제서야 잘 작동이 되었다. 

굉장히 허무하게 해결이 된 케이스라 반드시 이유라도 알고 넘어가야 잠을 편하게 잘것 같아 관련 글들을 찾아보니 
한번이라도 **AWS측에서 secret이 노출되었다는 연락을 받았으면 해당 유저에 락이 걸리는 것 같았다**. 

전에 팀원분께서 실수로 커밋에 민감 정보를 담은 파일을 포함 하셨었는데 그게 노출이 되었다고 메일을 
받은적이 있었다. 바로 해당 파일은 삭제했지만 AWS측에서 사전 조치를 자동으로 취하는 것 같았다.

