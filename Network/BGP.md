# Network Routing Protocol - BGP

## BGP(Border Gateway Protocol)

인터넷에는 AS(Autonomous System)이라는 자율 시스템이 존재한다. AS는 ISP 사업자(SKT, KT, LG)끼리 한 개 이상의 AS를 운영하고 있으며, 하나의 AS는 하나의 조직 단위로 볼 수 있다. 이러한 다른 자율 시스템(AS)간 연결이 되기 위해서는 내부와는 다른 방법으로 정보를 전달하여야 하는데, 이때 사용되는 방법이 BGP(Border Gateway Protocol) 이다. 일반적인 네트워크 환경에서는 다루게 될 일이 많지 않은 TCP 프로토콜로 유니캐스트로 라우팅 정보를 전송한다. 즉, 이 AS 들이 서로 라우팅 정보를 주고 받으며 통신을 가능하게 하는 대규모 네트워크의 통신을 위한 라우팅 프로토콜이다. 

<br>

동적 라우팅 프로토콜 중 대표적으로 RIP, OSPF, EIGRP와 같은 라우팅 프로토콜은 IGP(Interior Gateway Protocol)이나, BGP는 EGP(Exterior Gateway Protocol)에 속한다. IGP란 AS 내부에서 동작하는 라우팅 프로토콜이며, EGP는 서로 다른 AS 간 정보 교환용 라우팅 프로토콜이다. BGP 프로토콜은 IGP와는 다르게 다이렉트로 연결되어 있지 않은 장비와 BGP Peer(Neighbor) 관계 성립이 가능하다. 간단히 IGP와 EGP를 요약하자면 다음과 같다.


- **IGP** : 동일 AS 내의 BGP 라우터간의 Internal BGP
- **EGP** : 서로 다른 AS간 통신에 사용하는 External BGP

<br>

## 외부 및 내부 BGP

**eBGP(external BGP)**

- 서로 다른 AS 사이에서 BGP를 구성하는 경우

**iBGP(internal BGP)**

- 동일 AS 내에서 BGP를 구성하는 경우

<br>

## BGP 동작 방식

서로 다른 AS에 속한 BGP 라우터가 통신을 하기 위해서는 다른 망의 중계 없이 단일망으로 연결이 되어 있어야 한다. 만일, 하나의 AS내에 한 개 이상의 BGP 라우터가 있을 경우, BGP 라우터들은 AS에 대한 일관성 있는 정보와 AS 외부 통로 역할을 수행하기 위해 서로 인식하고 정보를 교환하여야 하며, BGP는 IGP와는 다르게 거리값(Distance Vector)와 같은 라우팅 알고리즘이나 목적지까지의 경로값을 전송하는 것이 아닌, 목적지까지 도달하기 위해 경유하는 AS의 순서를 전송한다. 각 라우터 엔트리는 하나의 AS를 거칠 때마다 AS Number 가 포함되며, AS-Path의 길이가 짧은 경로를 우선순위 경로로 선택하여 라우팅 테이블에 적재한다. 이러한 BGP 프로토콜에서 주고 받는 메세지 및 상태 변화 정보는 다음과 같다.

<br>

- **Idle(Idle → Active)** : BGP Session 연결 시도, Active 상태는 연결 실패
- **Open(Connect → Open Sent)** : BGP Peer를 형성하기 위한 메세지 수신
- **Update(Established → Update)** : BGP 테이블의 버전을 검사하고 변경이 있으면 업데이트한다. 업데이트 메세지는 자신의 BGP 정보를 상대에게 광고하기 위한 메세지 (단일 update 메세지 내에는 하나의 경로 정보만 포함된다.)
- **Notification** : 기존에 자신이 광고했던 경로에 문제가 발생한 경우 이를 알려주기 위한 메세지
- **Keepalive** : BGP Peer 상태를 확인하기 위한 메세지이며, 이 메세지는 자신이 살아 있다는 메세지를 Keep-Alive 주기로 BGP neighbor 에게 보낸다. 보통 Hole-time의 1/3으로 설정되며, Default는 60초이다. 다음으로 Hold-Down Timer 은 Default 로 180초이며, BGP neighbor 에서 이 시간동안 Keep-Alive 나 Update 메세지가 오지 않으면, 상대편 Peer 가 다운이 되었다고 판단하여  Peer 관계를 끊는다. 이 시간은 Default 180초이다.
