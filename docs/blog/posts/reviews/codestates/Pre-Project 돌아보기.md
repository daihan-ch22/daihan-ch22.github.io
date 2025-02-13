---
draft: false
date: 2022-09-10
categories:
  - self-review
---

    코드 스테이츠 부트캠프에서 진행했던 프리 프로젝트 개인 회고



## 프리 프로젝트 종료 
약 2주간의 pre-project가 끝이 나고 바로 main-project 기간이 시작되었다. 

메인 프로젝트를 시작하기 전에 프리 프로젝트를 진행하면서 느꼈던 점들을 한번 정리해보려고 한다.

우선 프리 프로젝트를 같이 무사히 넘길 수 있게 열심히 해주시고 부족한 팀장을 잘 믿고 따라와 주신  
44조 팀원분들에게 무수한 감사를 드리고 싶다. 

팀원 개개인의 실력차가 그렇게 나지 않았고 각자 실력 부분에서 부족한 부분이 많다는 공통된 생각을 처음에 공유했었다. 하지만 프리 프로젝트가 끝나고
나서 드는 생각은 비록 100% 완성을 이루진 못하였지만 열정, 의욕, 팀워크 부분에서는 정말 최고였다.  

다만 각자 열심히 했었기에 남는 아쉬움 또한 매우 많았다.  
<!-- more -->
--- 

## 팀 단위의 정리

팀끼리 정리해본 전체적인 측면에서의 잘했던 점과 아쉬웠던 점은 아래와 같다.  

### 좋았던 점
* 포기하지 않고 항상 적극적으로 프로젝트에 임한 것
* 매일 정해진 시간에 회의한 것
* 배포를 빨리 해본 것
* 원활한 소통을 이뤄낸 것
* 테이블 명세서를 빨리 만든 것
* 요구사항 정의서를 빨리 구현한 것
* 화면 정의서를 최대한 비슷하게 만들어본 것
* 프론트와 백간의 통신을 빠르게 확인해본 점
* 이슈에 대한 피드백이 빨랐던 점 <br>

### 아쉬웠던 점
* 사용자 경험을 구체적으로 생각해보지 않은 것
* 백엔드단에서 구현한 부분들을 전부 구현하지 못한 것
* 개개인의 진도를 체계적으로 관리하지 못한 점
* 요구사항 정의서의 구체성이 부족했다.
* ERD 설계에 대한 백엔드와 프론트간의 소통이 아쉬웠다.
* 테이블 명세서의 변화가 많았다. <br>


프리 프로젝트는 StackOverFlow를 클론 코딩해보는 것이었다. 아무리 클론 코딩이지만, 전원의 프로젝트 경험이
없는 상태에서 2주라는 짧은 시간 안에 처음부터 뭔가를 만들어 본다는 것은 정말 어려웠다. 

그렇기에 나는 약간 100% 완성이라는 목표보다는 우리 팀의 이번 프리 프로젝트의 방향은 완성을 못해도 좋으니 
부분적인 기능만이라도 백엔드와 프론트를 합쳐보고 배포까지 해보자는 큰 흐름에서의 목표를 설정했다. 
다행히 팀원들의 지지를 얻어냈었고, 배포까지 성공적으로 해보는 성과를 거두었다. 

특히 배포는 기한의 1주 전부터 내가 세팅을 해놓고 계속 배포를 지속적으로 하는 식으로 했는데 
이게 잘했던 것 같다. 당일날 했었으면 배포 못했을 것 같다. 

**내가 생각했을 때 우리 팀에서 가장 잘 한 점은 소통이 정말 잘 이루어졌다 라는 점이다.**

소통이 잘 이루어졌기 때문에, 에러에 대해서도 빠른 피드백이 오갈 수 있었고 에러에 대한 해결도 다 같이 찾아봄으로써 
시간을 많이 아낄 수 있지 않았나 싶다. 그리고 화기애애 한 분위기 속에서 개개인이 열심히 하니 팀의 사기 부분에서도 
매우 긍정적인 영향이 있었다고 생각한다.

다만 아쉬웠던 점은 내가 팀원 개개인의 일정관리를 체계화하지 못한 점이었다. 매일 오후 1시에 정기 회의를 했었지만 
구두로만 각자 뭘 하는지 공유하는 식으로 진행되었었는데, 이게 시각적으로 이루어지지 못했기 때문에 내가 팀원들의 
진도를 파악하기 어려웠다. <br>

따라서 메인 때는 체계적으로 하기 위해 노션을 써본다던가 Gantt Chart 같은 것들을 활용할 예정이다. <br>

---
## 셀프 평가


### 좋았던 점

* 미리 배포를 시도해서 개발 일정에 맞게 배포를 진행할 수 있었다.
* 기초적인 CRUD에 대해서는 처음보다 잘 이해가 되었음
* 아쉬웠던 점
* 로직 구현의 기여도가 낮았던 점
* 스프링과 jpa에 대한 이해도가 아직 많이 부족했음 특히 dto, mapper, entity연관관계)
* api 명세서를 RestDocs로 시도했었는데 제대로 완성하지 못한 점.
* 유효성 검증 안 해본 것
* 팀장으로서 팀원들 개개인의 일정 관리가 미흡했음(덜 체계적)
* 보안성을 신경 쓰지 못했음 (db정보의 암호화 X, http 사용)
* 배포를 진행했지만 팀원에게의 설명 미흡
* 프런트 부분의 흐름을 알지 못해서 프론트 팀원들에게 많은 도움을 줄 수 없었음<br>

### 개선할 부분

* 취약한 부분을 지속해서 보충해야 함
* API명세서를 swagger를 사용하여 도출해보면 좋을 듯
* Gantt Chart 같은 것을 사용해서 개발 일정 관리의 체계화
* 보안 부분 특히 신경 쓸 것
* 배포 관련해서 커뮤니케이션을 충분하게 할 것 (다른 팀원들이 할 수 있도록 충분한 정보 공유)
* 코드 참여도 및 이해도를 개선해야 함 <br>

**스스로에 대한 평가를 해보자면 배포를 시간 내에 매끄럽게 진행한 것 말고는 아쉬움이 대부분이었다.**<br>

---

특히 '백엔드 개발자'의 관점에서 봤을 때, 코드의 기여도가 많이 낮았고 관련 지식들이 매우 매우 부족하다고 느껴졌다.
위에도 적어놨듯이 dto와 mapper 그리고 jpa 부분이 정말 헷갈렸으며 이해가 아직도 잘 되지 않는다. 
지금은 프로젝트에서 팀장일을 하고 있지만 결국 나는 백엔드 개발자로 취업을 하는 것이 목표이기 때문에 
이 부분에 있어서는 좀 더 분발해야 된다는 생각이 든다. 

AWS와 DB를 관리하는 데 있어서도 보안성 문제에 별로 신경을 쓰지 못했다는 점도 매우 아쉬웠다. 
application.yml에 RDS 계정 정보가 이쁘게 깃허브 Repository에 올라가 있었고, AWS상에서의 보안설정도 다 열어놨고 
https를 적용해보지 못하는 등 보안이 많이 취약했다. 

커뮤니케이션 부분에서 아쉬웠던 점은 내가 배포를 진행하면서 이게 어떻게 흘러가는지 팀원에게 충분한 설명을 
못했다는 점이었다. 개인 AWS 계정을 사용하기 때문에 공유를 할 수가 없어서 대부분 내가 배포 관련해서는 전담해서 
진행했었는데 이게 결국 팀원들의 배포에 대한 이해도를 낮춘 것 같다. 

메인 프로젝트를 진행하면서 이번에 아쉽게 느껴졌던 부분들을 최대한 반영해볼 예정인데 메인 프로젝트가 끝나면 
지금처럼 또 아쉬운 점들이 나올 텐데 그러면서 조금씩 발전해가지 않을까 하는 기대가 크다. 

메인 프로젝트는 약 5주의 시간이 주어지게 되고 우리 팀은 주제 선정을 이미 해놨기 때문에 추석 연휴가 끝나게 되면
바로 명세서 등을 만들면서 본격적으로 시작이 될 것이다. 이번에는 멘토 분들도 팀별로 지정이 되었기 때문에
많이 여쭤보고 많이 배워보는 기간이 되었으면 한다.