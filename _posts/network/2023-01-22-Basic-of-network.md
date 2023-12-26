---
title: "Basics of network (TBC)"
categories:
- Network
toc: true
toc_label: "Basics of network"
toc_sticky: true
#header:
# teaser: "/assets/images/network.jpg"
---

{: .notice}
Basics of network terminology

## TCP/IP
_Transmission Control Protocol / Internet Protocol_

![TCP/IP](/assets/images/23-01-22-networking/tcpiposi.jpg "TCP/IP"){: .align-center}

현재 대다수의 응용 애플리케이션들이 인터넷과 통신하는 데 있어 TCP/IP 프로토콜을 사용한다. <br>

" / " 로 묶여있지만 TCP와 IP는 엄연히 다른 개념이다. 네트워크의 경우, 위 모델처럼 계층이 정의되어있고,<br> 각 레이어마다 하는 일과 책임 영역이 나뉘어 있기 때문에 하나로 묶어서 표현할 뿐이다.

모델에 계층이 나뉜 이유는 **각 Layer에 대한 캡슐화와 정보의 은닉이 가능**하기 때문이다.

|LEVEL|  MODEL  |  DESCRIPTION  |
|:--:|:--:|:---:|
|4|  APPLICATION |  사용자와 가장 가까운 계층 <br>동작을 위해 포트번호를 사용한다. <br> 주요 프로토콜: HTTP, DNS, SSH, FTP, Telnet, SMTP 등   |
|3|  TRANSPORT |  통신 노드 간의 연결 제어 <br> 프로세스 간의 신뢰성 있는 데이터 전송 <br> 주요 프로토콜: TCP, UDP  |
|2| INTERNET |OSI 모델의 Network Layer에 해당 (Level 3)  <br> 통신 노드 간 IP 패킷을 전송하는 기능과 라우팅 기능 담당 <br> 주요 프로토콜: IP, ARP, RARP  |
|1| NETWORK INTERFACE| OSI 모델의 Physical + Data Link layer에 해당 (Level 1, 2) <br> 물리적 주소인 MAC을 사용 <br> LAN과 패킷망 등에 사용 <br> 주요 프로토콜: Ethernet, wifi|


## IP
네트워크에 연결된 특정 PC의 주소를 나타내는 체계를 IP address(Internet Protocol address, IP주소) 라 한다. <br>

127.0.0.1처럼  (. )을 기준으로 4덩어리의 숫자로 구성된 IP 주소 체계를 IPv4라고 하며 (Internet Protocol version 4) IPv4는 각 덩어리마다 0~255 사이의 숫자로 나타낼 수 있고 총 2^32 (약 43억) 개의 IP주소를 표현할 수 있다.

다음의 주소는 이미 용도가 정해져 있어서 반드시 기억해야 한다.

```
localhost,  127.0.0.1 :
현재 사용 중인 로컬 PC를 지칭함

0.0.0.0,   255.255.255.255 :
로컬 네트워크에 접속된 모든 장치와 소통하는 주소 (broadcast address)  서버에서 접근 가능한 주소를 broadcast address로 지정하면
모든 기기에서 서버에 접근 가능하다.
```

시간이 흐르면서 IPv6도 나오게 되었는데 IPv4와는 다른 표기법을 가지고 있어 총 2^128개의 IP주소를 표현할 수 있다.
```FE80:CD00:0000:0 CDE:1257:0000:211 E:729C```

## PORT
* 포트( 통로 )를 통해 IP 주소로 접근할 수 있다.
* 이미 사용 중인 포트는 중복 사용이 불가능하다.
포트를 왜 쓰는지 이해하기 위해서는 데이터가 컴퓨터 간 네트워크를 통해 어떻게 이해하는지 큰 그림으로 먼저 이해할 필요가 있다.

서버 프로그램이 처음 실행되면, 지정된 포트번호에 할당된다. 그리고 이 서버를 사용하는 모든 클라이언트 프로그램들은 지정된 포트번호에 할당되어야 한다.

발신지 컴퓨터에서 출발한 패킷이 TCP/IP의 계층을 거쳐서 최종적인 목적지 IP를 가지고 있는 컴퓨터에 전송이 된다. <br>
이때, 도착지 컴퓨터에서 실행 중인 많은 프로그램들 중 누구에게 데이터를 전달해야 하는지 확실한 지표가 필요한데 프로그램의 논리적 주소인 Port 번호를 이용한다.
<br><br>
즉, 각각의 응용 프로그램에 Port 번호를 할당해서 데이터가 전송계층에서 어느 프로그램으로 갈지 구분하게 한다.
<br><br>
포트 번호는 0부터 65,535까지 사용할 수 있다. 그중에서 0부터 1024까지의 포트 번호는 주요 통신을 위한 규칙에 따라 이미 정해져 있는데 꼭 알아야 할 잘 알려진 포트번호는 아래와 같다.
<br>
* 22   : SSH
* 80   : HTTP
* 443 : HTTPS

## URL/URI
<br>
![URL/URI](/assets/images/23-01-22-networking/urluripng.png "https://danielmiessler.com/study/difference-between-uri-url/"){: .align-center}
_https://danielmiessler.com/study/difference-between-uri-url/_  
{: .text-center}

<br>
URL (Uniform Resource Locator)는 네트워크 상에서 웹 페이지, 이미지, 동영상 등의 파일 위치정보를 나타낸다.<br> 
URL은 크게 **[ scheme, host, url-path ]**의 3 부분으로 구분한다.<br>

URI (Uniform Resource Identifier)는 URL의 기본 3 요소인  [ scheme, host, url-path ]에 추가로 <br>
query와 bookmark를 포함한다. (예: https://www.google.com:80/search?q=Java) <br>

브라우저의 검색창을 클릭했을 때 나타나는 주소가 URI이다. URI는 URL을 포함하는 상위 개념이다. <br>
즉, URL은 URI이고 / URI는 URL이 아니다.  

|PARTS|  NAME OF ELEMENT  |  DESCRIPTION  |
|:--:|:--:|:---:|
|file://<br> http:// <br> https://|  **scheme** |  통신 프로토콜   |
|127.0.0.1 <br> www.naver.com|  **hosts** | 웹 페이지, 이미지, 동영상 등의 파일이 위치한 웹 서버, 도메인 또는 IP  |
|:80 <br> :443 <br> :3000| **port** | 웹 서버에 접속하기 위한 통로 |
|/search <br> /Users/username/Desktop| **url-path**| 웹 서버의 root 디렉토리로부터, 웹 페이미, 이미지, 동영상 등의 파일의 위치까지의 경로|
| q=Java | **query** | 웹 서버에 전달하는 추가 질문 |