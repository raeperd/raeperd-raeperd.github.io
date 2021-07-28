---
title: Subnetting
date: 2020-05-12
tags: ["network"]
---

- 큰 네트워크를 작은 네트워크로 나누는 과정
- 잘못된 서브넷 셋업이 많이들 실수하는 것이라 제대로 이해하고 넘어가는게 좋다.

Address class는 IP 주소가 가질 수 있는 주소를 의미있게 구분할 수 있다. 
만약 9.100.100.100 주소와 통신하고 싶다면, 인터넷 상의 core router는 9.100.100.100 주소가 9.0.0.0 class A 네트워크에 속한다는 것을 알 수 있다.
이후 core router는 이 메세지를 gateway router 에게 전달하는데 이 router 가 특정 네트워크의 입구이자 출구 역할을 한다.

그런데 다시 앞에서 배웠던 IP Address class를 생각해보면, class A 네트워크가 가질 수 있는 호스트는 생각보다 너무 많다. 

![](https://i.ibb.co/Tkj7Bwj/download.jpg)
하나의 router 에 모든 호스트가 연결되기에는 너무 많은 양이다.
￼
그래서 subnet 이 필요하다. 
![](https://i.ibb.co/XxZPVBf/download2.jpg)
![](https://i.ibb.co/80QTYb3/download3.jpg)

subnet 을 활용하면 큰 네트워크를 여러 작은 네트워크로 분할 할 수 있으며, 각각의 subnet 들은 입구이자 출구인 gateway router 를 가지게 된다.

### Subnet mask

network id 와 host id 에 대해서는 앞서 알아봤다. 더 자세하게 IP 주소를 구분하려면 Subnet ID 라는 새로운 개념이 필요하다. 
IP 주소는 단순히 32bit 숫자의 나열이다. 만약 subnet이 없다면, 특정 비트는 network ID 로 사용되고 나머지 특정 비트는 host ID로 사용될 것이다. subnet을 사용하면, host ID 를 나타내는 몇몇 비트들은 subnet ID 로도 쓰일 수 있다. 이 세가지 ID 를 하나의 32bit IP 주소에 표현함으로써 서로 다른 네트워크간에 정확하게 통신 할 수 있는 주소 체계를 만들 수 있다.
Internet level 에서, core router 들은 network ID 만을 이용해서 네트워크상에 적절한 gateway router에게 메세지를 전달한다. 이후 gateway router 들은 약간의 추가 정보를 가지고 datagram을 목적지, 혹은 다음 router 에게 전달한다. 마지막으로 host ID가 마지막 router 에 의해 사용되어 목적지인 하드웨어에 도착하게 된다.
subnet ID 는 subnet mask 라는 것을 통해 계산되는데 이 역시 IP 주소와 같이 32bit 숫자의 나열이다. subnet mask가 어떻게 동작하는데 이해하는 가장 좋은 방법은 IP 주소와 비교해보는 것이다.
￼
![](https://i.ibb.co/6n1pr1r/download4.jpg)
subnet mask 는 두가지 부분으로 나뉘는데, 앞의 연속된 1이 mask 자체의 역할을 하며 host ID 를 계산할 때 어디를 무시할 수 있는지를 나타낸다. 0이 있는 부분은 반대로 host ID 를 계산할때 필요한 부분을 나타낸다.

class A인 9.100.100.100의 경우 첫 바이트가 network ID를 나타낸다는 것을 알고 있다. network ID 가 아닌 부분 중에서, 1로 마스킹 된 부분이 subnet ID를 나타낸다. 그리고 나머지, 0으로 패딩된 부분이 host ID를 나타낸다.

1바이트가 256가지의 주소를 나타낼 수 있는데, subnet은 보통 가능한 host ID의 갯수보다 두개 적은 갯수 만큼의 host ID 를 가질 수 있다. 0은 보통 사용되지 않으며, 255는 subnet 상의 broadcast를 위해 사용된다. 두 주소를 할당해서 사용할 수는 없지만 사용가능한 주소는 256개로 보는 것이 맞다.

subnet mask는 32bit 이긴 하지만 나열에 규칙성이 있다. 앞의 마스크에 1이 몇개인지 명시하는 것만으로 어떤 subnet mask를 사용하는지 알 수 있다. 곧, 5bit 면 subnet mask를 구분하기에 충분하다. subnet mask는 아래와 같이 표기할 수 있다.
￼
![](https://i.ibb.co/2PsVwPn/download5.jpg)

IP 주소와 subnet mask를 AND 연산을 하면 host ID와 subnet ID를 확인할 수 있다. 이 연산을 한 컴퓨터는 자신의 네트워크 주소와 비교해 이 주소가 같은 네트워크 상에 있는지 알 수 있게 된다.

