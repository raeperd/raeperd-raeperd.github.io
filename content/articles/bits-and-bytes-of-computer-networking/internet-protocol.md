---
title: Internet Protocol
date: 2020-05-12
tags: ["network"]
---

 LAN 에서는 MAC 으로 기기간의 통신을 할 수 있다. 그런데 MAC 은 구조적으로 정렬된 주소는 아니다. 모든 네트워크 기기는 고유한 MAC 주소를 가지고 있지만, 특정 시점에 특정 MAC 주소를 가지는 기기가 지구상의 어디에 존재하는지는 알 방법이 없다. 분명히, 멀리 있는 기기간의 통신에서는 다른 종류의 해답이 필요하고, 이 문제를 3번째 Layer인  Network Layer에서 Internet Protocol 을 이용해 해결할 수 있다.

# IP Address
![](https://i.ibb.co/3CLtfyr/ip-address.jpg)
4 byte 구성된 주소

> IP addresses belong to networks, not to the devices attached to those network   

 내 노트북은 항상 같은 MAC 주소를 가지지만 IP는 장소에 따라 다른 값을 가지게 된다. IP 주소는 대부분 사용자가 모르는 사이, 자동으로 부여되는데  **Dynamic Host Configuration Protocol** 이라는 기술로부터 그것이 가능하다.

IP 주소는 **Dynamic IP address** 와 **Static IP address** 의 두가지 방법으로 할당되고 항상 그런 것은 아니지만 보통 서버의 경우 Static, Client의 경우 Dynamic 한 방법으로 부여된다.

> In most cases, static IP addresses are reserved for servers and network devices, while dynamic IP addresses are reserved for clients  


## IP Datagrams and Encapsulation
Ethernet layer의 패킷들이 Ethernet frame 의 형태를 가진 것처럼, Network layer의 패킷들도 특정한 형태를 가지게 된다. 이를 IP Datagram 이라 부른다. Ethernet의 경우보다 더 많은 정보를 가지고 있다.
Ethernet frame과 마찬가지로 비트단위로 철저하게 정의되어 있다. IP Datagram 또한 header와 payload 로 크게 구분할 수 있다.

![](https://i.ibb.co/F5TbCrJ/ip-datagram-header.jpg)
￼
### Version
* Internet protocol 의 버전이 무엇인지 명시
* 가장 보편적인 버전은 4이다. 이를 보통 IPv4라 부른다. 

### Header Length
* 전체 헤더의 길이
* IPv4의 경우 거의 항상 20byte 를 가리키게 된다. 이 크기가 IPv4 헤더가 가질 수 있는 최소의 크기이다.

### Service Type
* 서비스의 품질(QOS: Quality of Service)을 나타내는 지표
* 라우터가 어떤 IP Datagram이 더 중요한지 판단하는 지표가 된다.

### Total Length
* 말 그대로 전체 IP Datagram의 전체 길이를 나타낸다.
* 16bit 사이즈를 가지므로, IP Datagram 이 가질 수 있는 최대의 크기는 2^16 = 65535 이다

> If the total amount of data that needs to be sent is larger than what can fit in a single datagram, the IP layer needs to split this data up into many individual packets.  

### Identification
* 전체 길이가 2^16 보다 커지는 경우, 패킷은 여러번 나뉘어 보내져야한다. 이때 이부분이 같다면 같은 정보를 나타낸다는 것을 의미한다.

### Flag / Fragment offset
* Datagram 이 분할(Fragmentation) 될 수 있는지 혹은 이미 분할되어 있는지를 나타낸다.
- Fragmentation
	- The process of taking a single IP datagram and splitting it up into several smaller datagrames
* Datagram 의 최대 크기는 보통 보편적인 옵션을 공통적으로 사용하지만 이를 다르게 설정할 수도 있다.
	* 이렇게 되면 크게 설정 되어있는 곳에서 작게 설정되어 있는 곳으로 패킷이 움직일때는 패킷이 분할될 필요가 있다.
	* 이 필드는 받는 쪽에서 올바른 순서로 분할된 패킷을 다시 합치는데에 사용된다.

### TTL (Time to live)
- An 8 bit field that indicates how many router hops a datagram can traverse before it’s thrown away

![](https://i.ibb.co/b3gqFvd/ttl.jpg)
* Datagram이 버려지기전에 몇번이나 라우터를 거칠 수 있는지를 나타낸다.
* 새로운 라우터에 Datagram이 지나갈때 마다 이 값이 하나씩 감소되며, 0이 되는 순간 버려진다.
* 라우팅이 잘못 설정되었을때 무한히 돌아다니는 패킷이 없도록 하려면 이런 필드가 필요하다.
* 이 필드가 네트워크 관련된 설정의 디버깅에 유용하게 사용될 수 있다.

### Protocol
* 어떤 Transport Layer Protocol 이 사용되는 지 명시한다
* 보통 TCP 나 UDP 를 많이 쓴다.

### Header Checksum
* IP datagram header의 체크섬
* **TTL이 라우터 마다 변경되기 때문에 이필드도 매번 다시 계산되어야 한다.**

### Source IP Address / Destination IP Address
* 말 그대로의 의미
* IP 주소가 32bit 이기 때문에 당연히 각각 32bit 를 가진다. 

### Options
* 테스트 용도로 주로 사용되는 필드
* 있어도되고 없어도 되며 길이도 자유롭다.

### Padding
* Options 필드를 포함에서 전체 헤더의 크기가 일관되도록 0으로 패딩한다. 

### Encapsulation
￼![](https://i.ibb.co/7Q0x9zv/message-encapsulation.jpg)
- 전체 IP datagram은 Ethernet frame의 payload에 속한다.
- 마찬가지로 TCP packet은 IP Datagram의 payload에 속하게 된다.
- 이렇게 하나의 layer는 그 상위의 layer를 포함하며 가지는 구조를 가진다.
- 각각의 layer들은 상위 layer에게 필요한 구조를 가지게 된다.

## IP Address Classes
![](https://i.ibb.co/Wyh8FC7/ip-address-classes.jpg)
- IP Address는 network id 와 host id 로 구분될 수 있다.
￼
### Address class system
- A way of defining how the global IP address space is split up
- IP Address가 어떻게 구분되는지를 정의한 시스템.

![](https://i.ibb.co/8NFcgTC/ip-address-class-system.jpg)
- Class A
	- First 1 bytes of network ID, 3 bytes of host ID
- Class B
	- First 2 bytes of network ID, 2 bytes of host ID
- class C 
	- First 3 bytes of network ID, 1 bytes of host ID
- IP 주소를 보는 것만으로 어떤 클래스 인지 확인할 수 있다.
- D class 와 E class는 특수한 목적에 사용되는 클래스로 비교적 덜 중요하다. 
	- D는 멀티 캐스팅, E는 테스트를 하는 목적으로 사용된다.
	- 실제로 이런 클래스 시스템은 대부분 **CIDR (Classless inter-domain routing)** 이라는 방법으로 대체되었다.
	 - 그럼에도 Address class system은 아직 많이 있고 네트워크 시스템 전반을 이해하는데 큰 도움이 되기 때문에 이해하고 있을 필요가 있다.


# Address Resolution Protocol
이제까지 MAC이 어떻게 쓰이는지와 Data Link Layer가 어떻게 사용되는지를 각각 알아 보았는데, 이제 이 두 시스템이 서로 연결되는지 알아볼 차례다.  

ARP는 특정 IP 주소를 통해 하드웨어의 주소를 찾기 위해 사용되는 프로토콜이다.  

IP Datagram은 Ethernet frame 의 내부에 속하게 되므로, 패킷을 전송하는 쪽에서 패킷을 완성하려면 전송하는 쪽의 MAC 주소를 알고 있어야 한다. 거의 모든 네트워크 장비들은 ARP Table 을 가지고 있는데 이는 IP 주소와 MAC 주소를 연결한 테이블이다. ARP Table에 주소가 있다면 그렇게 보내면 된다. 없다면 어떻게 해야할까￼

1. 10.20.30.40 주소로 보내려는데 해당하는 MAC 주소를 알 수 없는 경우 Broadcast ARP Message를 보낸다.
2. 10.20.30.40 IP 주소를 가진 기기가 이를 받으면 ARP Response 를 보내게 되는데 이 응답을 바탕으로 해당하는 MAC 주소를 알 수 있게 된다.
3. 이제 이 정보를 ARP Table에 저장하면 다음번에 이 주소가 필요한 경우 같은 과정을 반복할 필요가 없어진다.
4. ARP Table은 자주 파기 되기 때문에 한번 저장된 주소가 앞으로도 저장되어 있을것이라고 생각할 수 없다.