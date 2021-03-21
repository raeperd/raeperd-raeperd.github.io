---
title: "Subnetting"
date: 2020-04-14T00:49:35+09:00
tags : ["network"]
---

# INTRO

 필요한 지식인거 알고, 해야한다는걸 아는데 왜 이렇게 집중이 안될까. 첫주차 과정을 끝내고 거의 한달 뒤에서야 두번째 주의 내용을 얼추 마무리 했다. 이번주의 내용이 좀더 많기도 하고 정리할 내용들이 많아 시간이 더 오래 걸린 것도 있지만 결정적으로 이번 한달 내가 너무 게으르게 행동했던 것 같다. 막상 끝내고 보니 확실히 막연하게만 알고있던 내용들이 한번씩 정리되는 느낌이라 개운하기는 하다. 더 잘하자. 


# CONTENTS



## The Network Layer 

![tcp-ip-5-layer-network-layer](/images/tcp-ip-5-layer-network-layer.jpeg)

 LAN 에서는 MAC 으로 기기간의 통신을 할 수 있다. 그런데 MAC 은 구조적으로 정렬된 주소는 아니다.  모든 네트워크 기기는 unique 한 MAC 주소를 가지고 있지만, 특정 시점에 특정 MAC address 를 가지는 기기가 지구상의 어디에 존재하는지는 알 방법이 없다. 분명히, 멀리 있는 기기간의 통신에서는 다른 종류의 해답이 필요하고, 이를 3번째 Layer 인 network layer 의 Internet Protocol 을 이용해 해결할 수 있다. 



### IP Addresses

![ipv4-address-notation](/images/ipv4-address-notation.jpeg)

IP 주소는 Network Layer에 속한다. IP 주소 자체가 네트워크 기기와 결합된 것은 아니다. 내 노트북은 항상 같은 MAC 주소를 가지지만 IP는 장소에 따라 다른 값을 가지게 된다. IP 주소는 대부분 자동으로 부여되는데 이때 Dynamic Host Configuration Protocol 이라는 기술로부터 그것이 가능하다. 



IP 주소는 **Dynamic IP address** 와 **Static IP address** 의 두가지 방법으로 할당되고 항상 그런 것은 아니지만 보통 서버의 경우 Static, Client의 경우 Dynamic 한 방법으로 부여된다. 



### IP Datagrams and Encapsulation 

Ethernet layer의 패킷들이 Ethernet frame 의 형태를 가진 것처럼, Network layer의 패킷들도 특정한 형태를 가지게 된다. 이를 IP Datagram 이라 부른다. Ethernet의 경우보다 더 많은 정보를 가지고 있다. 

Ethernet frame과 마찬가지로 비트단위로 철저하게 정의되어 있다.  IP Datagram 또한 header와 payload 로 크게 구분할 수 있다.

![ip-datagram](/images/ip-datagram.jpeg)

#### Version

- Internet protocol 의 버전이 무엇인지 명시
- 가장 보편적인 버전은 4이다. 이를 보통 IPv4라 부른다. 



#### Header Length

- 전체 헤더의 길이 
- IPv4의 경우 거의 항상 20byte 를 가리키게 된다. 이 크기가 IPv4 헤더가 가질 수 있는 최소의 크기이다.



#### Service Type 

- 서비스의 품질(QOS: Quality of Sevice)을 나타내는 지표
- 라우터가 어떤 IP Datagram이 더 중요한지 판단하는 지표가 된다. 



#### Total Length 

- 말 그대로 전체 길이를 나타낸다. 
- 16bit 사이즈를 가지므로 IP Datagram 이 가질 수 있는 최대의 크기는 2^16 = 65535 이다



#### Identification

- 전체 길이가 2^16 보다 커지는 경우, 패킷은 여러번 나뉘어 보내져야한다. 이때 이부분이 같다면 같은 정보를 나타낸다는 것을 의미한다. 



#### Flag  / Fragment offset

- Datagram 이 분할(Fragmentation) 될 수 있는지 혹은 이미 분할되어 있는지를 나타낸다. 
- Datagram 의 최대 크기는 보통 보편적인 옵션을 공통적으로 사용하지만 이를 다르게 설정할 수도 있다. 
  - 이렇게 되면 크게 설정 되어있는 곳에서 작게 설정되어 있는 곳으로 패킷이 움직일때는 패킷이 분할될 필요가 있다.
  - 이 필드는 받는 쪽에서 올바른 순서로 분할된 패킷을 다시 합치는데에 사용된다. 



#### TTL (Time to live)

- Datagram이 버려지기전에 몇번이나 라우터를 거칠 수 있는지를 나타낸다. 
- 새로운 라우터에 Datagram이 지나갈때 마다 이 값이 하나씩 감소되며, 0이 되는 순간 버려진다. 
- 라우팅이 잘못 설정되었을때 무한히 돌아다니는 패킷이 없도록 하려면 이런 필드가 필요하다. 
- 이 필드가 네트워크 관련된 설정의 디버깅에 유용하게 사용될 수 있다. 



#### Protocol

- 어떤 Transport Layer Protocol 이 사용되는 지 명시한다
- 보통 TCP 나  UDP 를 많이 쓴다. 



#### Header Checksum

- IP datagram header의 체크섬
- TTL이 라우터 마다 변경되기 때문에 이필드도 매번 다시 계산되어야 한다. 



#### Source IP Address / Destination IP Address

- 말 그대로의 의미



#### Options 

- 테스트 용도로 주로 사용되는 필드
- 있어도되고 없어도 되며 길이도 자유롭다. 



#### Padding

- Options 필드를 포함에서 전체 헤더의 크기가 일관되도록 변경해다. 



![encapsulation-view-of-packets](/images/encapsulation-view-of-packets.jpeg)

전체 IP datagram은 Ethernet frame의 payload에 속한다. 

마찬가지로 TCP packet은 IP Datagram의 payload에 속하게 된다.

이렇게 하나의 layer는 그 상위의 layer를 포함하며 가지는 구조를 가진다.

각각의 layer들은 상위 layer에게 필요한 구조를 가지게 된다. 



### IP Address Classes

IP Address는 network id 와 host id 로 구분될 수 있다. 

![ip-address-classes](/images/ip-address-classes.jpeg)



### Address class system

IP Address가 어떻게 구분되는지를 정의한 시스템이다. 

![address-class-system](/images/address-class-system.jpeg)

IP 주소를 보는 것만으로 어떤 클래스 인지 확인할 수 있다. 



A 클래스는 첫 비트가 0

B 는 10

C 는 110

D는 1110



D와 E는 특수한 목적에 사용되는 클래스 이다. 

D는 멀티 캐스팅, E는 테스트를 하는 목적으로 사용된다. 



실제로 이런 클래스 시스템은 대부분 **CIDR (Classless inter-domain routing)** 이라는 방법으로 대체되었다. 

그럼에도 Address class system은 아직 많이 있고 네트워크 시스템 전반을 이해하는데 큰 도움이 되기 때문에 이해하고 있을 필요가 있다. 



### Address Resolution Protocol

이제까지 MAC이 어떻게 쓰이는지와 Data Link Layer가 어떻게 사용되는지를 각각 알아 보았는데, 이제 이 두 시스템이 서로 연결되는지 알아볼 차례다. 



ARP는 특정 IP 주소를 통해 하드웨어의 주소를 찾기 위해 사용되는 프로토콜이다. 



IP Datagram은 Ethernet frame 의 내부에 속하게 되므로, 패킷을 전송하는 쪽에서 패킷을 완성하려면 전송하는 쪽의 MAC 주소를 알고 있어야 한다. 



거의 모든 네트워크 장비들은 ARP Table 을 가지고 있는데 이는 IP 주소와 MAC 주소를 연결한 테이블이다. 

ARP Table에 주소가 있다면 그렇게 보내면 된다. 없다면 어떻게 해야할까



![When the network interface that's been assigned an IP of 10.20.30.40 receives this ARP broadcast, it sends back what's known as an ARP response.](/images/arp-broadcast-example.jpeg)

10.20.30.40 주소로 보내려는데 해당하는 MAC 주소를 알 수 없는 경우 Broadcast ARP Message를 보낸다.



![When the network interface that's been assigned an IP of 10.20.30.40 receives this ARP broadcast, it sends back what's known as an ARP response.](/images/arp-response-example.jpeg)

10.20.30.40 IP 주소를 가진 기기가 이를 받으면 ARP Response 를 보내게 되는데 이 응답을 바탕으로 해당하는 MAC 주소를 알 수 있게 된다. 

이제 이 정보를 ARP Table에 저장하면 다음번에 이 주소가 필요한 경우 같은 과정을 반복할 필요가 없어진다. 

ARP Table은 자주 파기 되기 때문에 한번 저장된 주소가 앞으로도 저장되어 있을것이라고 생각할 수 없다. 





## Subnetting

큰 네트워크를 작은 네트워크로 나누는 과정 

잘못된 서브넷 셋업이 많이들 실수하는 것이라 제대로 이해하고 넘어가는게 좋다. 

Address class는 IP 주소가 가질 수 있는 주소를 의미있게 구분할 수 있다. 만약 9.100.100.100 주소와 통신하고 싶다면 인터넷 상의 core router는 9.100.100.100 주소가 9.0.0.0 class A 네트워크에 속한다는 것을 알 수 있다. 

이후 core router는 이 메세지를 gateway router 에게 전달하는데 이 router 가 특정 네트워크의 입구이자 출구 역할을 한다. 



그런데 다시 앞에서 배웠던 IP Address class를 생각해보면, class A 네트워크가 가질 수 있는 호스트는 생각보다 너무 많다.  하나의 router 에 모든 호스트가 연결되기에는 너무 많은 양이다. 

![This all makes sense until you remember that a single class A network contains 16,777,216 individual IPs.](/images/address-class-system-too-many-hosts.jpeg)



그래서 subnet 이라는게 필요하다. subnet 을 활용하면 큰 네트워크를 여러 작은 네트워크로 분할 할 수 있으며 각각의 subnet 들은 입구이자 출구인 gateway router 를 가지게 된다. 



### Subnet mask

network id 와 host id 에 대해서는 앞서 알아봤다. 더 자세하게 IP 주소를 구분하려면 Subnet ID 라는 새로운 개념이 필요하다. IP 주소는 단순히 32bit 숫자의 나열이다. 만약 subnet이 없다면, 특정 비트는 network ID 로 사용되고 나머지 특정 비트는 host ID로 사용될 것이다. subnet을 사용하면, host ID 를 나타내는 몇몇 비트들은 subnet ID 로도 쓰일 수 있다. 이 세가지 ID 를 하나의 32bit IP 주소에 표현함으로써 서로 다른 네트워크간에 정확하게 통신 할 수 있는 주소 체계를 만들 수 있다. 



Internet level 에서, core router 들은 network ID 만을 이용해서 네트워크상에 적절한 gateway router에게 메세지를 전달한다. 이후 gateway router 들은 약간의 추가 정보를 가지고 datagram을 목적지, 혹은 다음 router 에게 전달한다. 마지막으로 host ID가 마지막 router 에 의해 사용되어 목적지인 하드웨어에 도착하게 된다. 



subnet ID 는 subnet mask 라는 것을 통해 계산되는데 이 역시 IP 주소와 같이 32bit 숫자의 나열이다. subnet mask가 어떻게 동작하는데 이해하는 가장 좋은 방법은 IP 주소와 비교해보는 것이다.

![The part with all the zeros tells us what to keep.](/images/subnetmask-example.jpeg)



subnet mask 는 두가지 부분으로 나뉘는데, 앞의 연속된 1이 mask 자체의 역할을 하며 host ID 를 계산할 때 어디를 무시할 수 있는지를 나타낸다. 0이 있는 부분은 반대로 host ID 를 계산할때 필요한 부분을 나타낸다. 



class A인 9.100.100.100의 경우 첫 바이트가 network ID를 나타낸다는 것을 알고 있다. network ID 가 아닌 부분 중에서, 1로 마스킹 된 부분이 subnet ID를 나타낸다. 그리고 나머지, 0으로 패딩된 부분이 host ID를 나타낸다. 



1바이트가 256가지의 주소를 나타낼 수 있는데, subnet은 보통 가능한 host ID의 갯수보다 두개 적은 갯수 만큼의 host ID 를 가질 수 있다. 0은 보통 사용되지 않으며, 255는 subnet 상의 broadcast를 위해 사용된다. 두 주소를 할당해서 사용할 수는 없지만 사용가능한 주소는 256개로 보는 것이 맞다. 



subnet mask는 32bit 이긴 하지만 나열에 규칙성이 있다. 앞의 마스크에 1이 몇개인지 명시하는 것만으로 어떤 subnet mask를 사용하는지 알 수 있다. 곧, 5bit 면 subnet mask를 구분하기에 충분하다.  subnet mask는 아래와 같이 표기할 수 있다.

![The entire IP and subnet mask can be written now as 9.100.100.100/27.](/images/subnetmask-notation.jpeg)





![The computer that just performed this operation can now compare the results with its own network ID to determine if the address is on the same network or a different one.](/images/subnetmask-and-operation.jpeg)

IP 주소와 subnet mask를 AND 연산을 하면 host ID와 subnet ID를 확인할 수 있다. 이 연산을 한 컴퓨터는 자신의 네트워크 주소와 비교해 이 주소가 같은 네트워크 상에 있는지 알 수 있게 된다. 





### CIDR (Classless Inter-Domain Routing)

 

Address class를 이용해 IP 주소를 잘 나눌 수 있었다. Subnetting은 Address class로 나누어진 network 를 기존의 규칙을 유지하면서 네트워크를 더 작게 분할할 수 있게 해줬다. 그런데 인터넷이 더 커지면서 Subnetting 도 충분하지 않게 되었다. 



![254 hosts in a class C network is too small for many use cases, but the 65,534 hosts available for use in a class B network is often way too large.](/images/address-class-system-too-many-hosts-on-classb.jpeg)

254 개의 호스트를 사용할 수 있는 class C 네트워크 보다는 많고 65534 개의 호스트를 사용할 수 있는 class B 네트워크는 너무 많다고 느껴지면 어떻게 할까? class C 네트워크 여러 개를 사용하는 방법이 있다. 이런 현상이 반복되면, 라우팅 테이블의 class C 여러 네트워크들은 다른 주소를 가지고 있지만 실제로는 같은 장소로 라우팅 될 것 이다. 



CIDR 가 이를 해결해 줄 수있다. 

demarcation point

- To describe where one network or system ends and another one begins



CIDR 에서는 네트워크 아이디와 subnet ID는 하나로 통합된다.

CIDR는 기존의 address class 의 규칙을 무시한 새로운 규칙이다. 단순히 address class 를 무시했기 때문에, subnet mask로 사용되는 만큼 network ID를 부여할 수있고, 그만큼 호스트 아이디를 가질 수 있다. 규칙이 간단해서 이해하고 적용하기도 쉬운반면 기존의 address class 보다 효율적인 면도 있다.



![512 minus two, 510 hosts.](/images/calculation-for-subnetting-using-cidr.jpeg)



클래스 C를 두개 쓰면 508개의 호스트를 가질 수 있지만 /23 network 는 클래스 C보다 단지 하나의 비트를 호스트에 더 할당함으로써 510개의 호스트를 가질 수 있다. 



## Routing 



### Basic Routing concept



#### Router

네트워크 트래픽을 목적지로 전달시키는 네트워크 장비



라우터는 최소한 두개의 네트워크 인터페이스와 연결되어 있어야 한다. 그래야 연결해 줄 수 있으니까. 라우터는 기본적으로 4 단계의 동작을 한다. 

![Four, the router forwards that out though the interface that's closest to the remote network.](/images/basic-routing.jpeg)

1. 연결된 네트워크 인터페이스들 중 하나로부터 패킷을 받는다.
2. 패킷의 도착 IP 주소를 확인한다.
3. 라우팅 테이블에서 IP 주소를 확인한다.
4. 트래픽을 도착지로 전달한다.



![On Network A, it has an IP of 192.168.1.1 and on Network B, it has an IP of 10.0.254.](/images/routing-example-with-two-network.jpeg)

IP 주소는 네트워크에 속해있다. IP 주소 하나 하나가 하나의 네트워크를 의미하는 것이 아니다. 

192.168.1.100 주소의 컴퓨터에서 10.0.0.10 의 주소로 패킷을 보낸다. 이때 컴퓨터 A는 자신이 속한 네트워크에 10.0.0.10 의 주소가 없다는 것을 알 수 있고, 이 패킷을 gateway router 에게 전달하는데 이 때의 전송은 gateway의 MAC 주소를 통해 한다. 

![reminder for encapsulation](/images/encapsulation-view-of-packets.jpeg)

그렇게 라우터는 Data link layer 의 패킷을 받게 되는데, 한번 받은 이후에는 Data-link layer의 정보를 잘라낸다. (Ethernet header와 Ethernet footer) 

IP datagram 에는 10.0.0.10의 주소가 기록되어있는데 라우터는 routing table에서 Network B가 올바른 목적지라는 것을 알 수 있다.  실제로 라우터워 Network B는 직접 연결되어 있기 때문에 router는 10.0.0.10의 MAC 주소까지 ARP 테이블에 가지고 있다. 



이제 라우터는 IP datagram의 모든 데이터를 복사하고, TTL 필드의 값을 하나 낮추며, 다시 체크섬을 계산한다. 이를 새로운 Ethernet frame 으로 만드는데, 이번에 Ethernet frame의 Source MAC address 는 라우터 자신의 주소가 되고, destination MAC address는 도착지의 MAC 주소가 된다. 



이정도가 기본적인 라우팅 과정인데, 좀 더 복잡한 경우는 어떻게 될까?



![This time around our computer at 192.168.1.100 wants to send some data to the computer that has an IP of 172.16.1.100.](/images/routing-example-with-three-network.jpeg)

이번에는 192.168.1.100 에서 172.16.1.100 으로 패킷을 전달하려고 한다고 해보자. 앞의 과정과 같이 Network B 까지 같은 과정으로 패킷이 전달 됬을 것이다. 그러나 Router A는 이전과 달리 172.16.1.100 의 주소를 Network B에서 찾지 못한다. 다른 라우터를 통해야 한다는 것을 알게된 Router A는 Router B의 IP 주소로 같은 패킷을 전달한다. 마찬가지로 TTL을 감소시키고 다시 체크섬을 계산하는 과정을 거친다. Router B에 도착한 패킷은 앞에서와 같은 원리로 도착지에 전달될 수 있다. 



실제로는 라우터에 더 많은 네트워크가 연결되어 있을 수 있고, 하나의 패킷이 전달되는 경로는 유일하지 않을 수 있다. 라우터가 어떻게 효율적으로 패킷의 경로를 알 수 있는지는 또 다른 복잡한 알고리즘들이 필요하겠지만 핵심은 같다. 



### Routing tables



최신의 운영체제들은 대부분 라우팅 테이블을 가지고 있다. 그래서 컴퓨터와 수동으로 업데이트 되는 라우팅 테이블만 있다면 라우터를 만들어볼 수도 있다. 라우터를 어떻게 만드느냐에 따라 종류는 많지만 몇가지 공통되는 특징을 가지고 있다. 



보통의 라우팅 테이블은 4개의 컬럼을 가진다. 

1. Destination network

   - 여러 컬럼에 나눠 저장될수도 있고, 하나의 컬럼에 저장될 수도 있으나 본질은 같음
   - catchall entry라는 것을 가지고 있는데 ARP 테이블에 없는 destination address의 패킷을 전달하는 곳 
2. Next hop

   - 도착지를 묻는데 사용될 수 있는 다음 라우터
   - 목적지까지 가기위한 바로 다음의 라우터 또는 경로
3. Total hops
   - 출발지와 도착지 사이의 네트워크 장치의 수 
4. Interface 
   - 라우터가 대상 네트워크와 트래픽을 일치시키기 위해 어떤 인터페이스를 사용해야하는지 알아야 한다. 


![Total hops, this is the crucial part to understand routing and how routing tables work, on any complex network like the Internet, there will be lots of different paths to get from point A to point B. Routers try to pick the shortest possible path at all times to ensure timely delivery of data but the shortest possible path to a destination network is something that could change over time, sometimes rapidly, intermediary routers could go down, links could become disconnected, new routers could be introduced, traffic congestion could cause certain routes to become too slow to use.](/images/routing-world-wide.jpeg)

라우터는 최적의 경로로 데이터를 전달하고자 하겠지만, 사실 이게 쉬운일은 아니다. 최적의 경로를 찾는다 한들 이는 변할 수 있다. 새로운 라우터가 추가되거나, 없어질 수도 있으니까. 트래픽이 몰려서 정체현상 같은게 일어날수도 있고. 



라우터는 위의 정보들과 이웃 라우터들로 부터 새로운 정보를 전달받으면서 더 나은 경로를 찾으려고 한다. 

 

### Routing Protocol

 간단한 라우팅 원리를 알아봤는데 라우팅의 진짜 마법은 라우터가 어떻게 도착지 경로를 알고, 다음 경로를 계산하는지이다. 라우터들은 이를 위해, 서로의 정보를 공유하는데 이때 사용되는 프로토콜이 Routing Protocol 이다.



#### Interior gateway protocol

- 하나의 Autonomous system 내부에서 사용되는 프로토콜 
- Autonomous system 은 하나의 network operator에 의해 제어되는 네트워크들의 모임 
  - Internet service provider나 큰 규모의 회사들
  
  
  
![In order to reach network X in the fastest way, it should forward traffic through its own interface 1 to router B.](/images/distance-vector-protocol-example.jpeg)

- Distance-vector protocol
  - total hops 와 함께 자신의 이웃 라우터들에게 라우팅 테이블을 공유
  - 내가 아는 total hops 보다 니가 아는거 + 1 이 더 적구나?
  - 그럼 너한테 보내면 되겠네?
  - 많은 정보를 알고 있는건 아니라 네트워크의 변화에 적응하는데 시간이 걸릴것
  - [RIP (Routing Internet Protocol)](https://en.wikipedia.org/wiki/Routing_Information_Protocol) 같은 것이 대표적



![The information about each router is propagated to every other router on the autonomous system.](/images/link-state-routing-protocol-example.jpeg)

- Link state routing protocol

  - 라우터의 상태를 모두 서로 공유하고 있는상태 
  - Distance vector protocol 보다 더 많은 정보를 가지고 있고 네트워크의 변화에 유연하다. 
  - 더 많은 메모리와 컴퓨팅 파워가 필요하지만 최근의 기술로 이게 더 많이 쓰이게 됬다. 
  - [OSPF(Open Shortest Path First)](https://en.wikipedia.org/wiki/Open_Shortest_Path_First) 같은 것이 대표적

  
  
#### Exterior gateway protocol

- Exterior gateway protocols are used to communicate data between routers representing the edges of an autonomous system. 
- 이게 중요한 역할을 한다. 
- 하나의 프로토콜 만이 존재하는데, 이런 종류의 데이터 통신에는 어느정도의 합의가 필요했던 것 같다. [BGP (Broader Gateway Protocol)](https://en.wikipedia.org/wiki/Border_Gateway_Protocol) 라고 한다.
  
  
  
### IANA (Internet Assigned Numbers Authority)

- IP 할당과 같은 것을 관리하는 것을 도와주는 비영리 단체 
- 이런 단체가 없으면 본인들 마음대로 IP를 쓰게될텐데 길찾기가 영 쉽지 않을 것이다.
- IP 할당뿐만 아니라 ASN(Autonomous System Number)의 할당도 관리한다. 
  - IP 주소처럼 32bit로, 하나의 Autunomous System에 할당되는 고유한 숫자다. 
  - IP 처럼 나뉜 숫자가 아니라 한번에 쓰이는데 IP 처럼 보통 사람의 눈에 많이 띌 숫자가 아니기 때문



Exterior gateway protocol을 이해해야만 해결할 수 있는 이슈는 많지 않을 것이다. Internet service provider 같은 곳에서 일하게된다면 많이 쓰이겠지만 일반적인 개발자는 그냥 그런게 있지~ 하고 받아들이고 사용하면 될 것 같다. 



### Non-Routable Address Space

과거에서부터 인터넷이 더 발전하면 32 bit IP 주소는 부족할 것으로 예상됐다. 1996년에 RFC 1918이 설립되었고 이곳에서 세상의 인터넷이 잘 동작하도록 여러가지 규칙들을 만드는 기능을 한다.

Non-Routable Address Space는 말그대로 라우팅이 불가능한 주소이다. 모든 네트워크 장비가 세상의 모든 네트워크 장비와 송수신이 가능할 필요는 없기 때문에 이런 주소가 필요하게 됬다. NAT라는 기술을 통해 외부와 통신할 수 있지만 나중에 배워보도록 하자. 일단 이 주소들은 통신이 불가능하다고 생각하자. 

어차피 외부와 소통할 수 없는 주소이기 때문에 어떤 사람이든지 사용할 수 있는 주소다. 크게 3가지 주소가 있다.



- 10.0.0.0/8
- 172.16.0.0/12
- 192.168.0.0/16



누구나 개인 혹은 내부망으로 이런 주소들을 사용할 수 있다. 내부 게이트웨이 프로토콜은 이러한 주소 공간을 라우팅 할 수 있지만 exterior gateway protocol 들은 라우팅 할 수없다. 
  

#### 왜 틀린거지?? 
![why-not-1](/images/why-not-1.png)

다시봐도 맞는거같은데..?


# Referenced

[computer-networking](https://www.coursera.org/learn/computer-networking)