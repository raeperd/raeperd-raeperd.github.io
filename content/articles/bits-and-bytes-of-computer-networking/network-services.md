---
title: Network Services
date: 2020-06-21
tags: ["network"]
---

# INTRO

기초가 참 힘들다. 뭐든지 기초가 참 힘든거 같다. 퇴근하고 공부하는게 참 힘든일이긴 한데 그냥 코딩하는거면 어떻게든 재미를 찾을 수 있을 것 같은데 기초 공부라는게 되게 지루하다. 농구선수가 운동장 달리고 푸쉬업 하는 것처럼 기초는 중요하지만 귀찮고 재미없고 따분하기만 하다. 그래도 해야지 뭐 하면서 또 하게 된다. 잘 좀 하자.

# CONETNS

## Name Resolution

### Why do we need DNS?

우리가 접속해야하는 사이트마다 IP 주소를 기억해야 한다고 생각하면 참 귀찮을 것이다. 사람들은 숫자를 기억하는 것보다 단어를 더 잘 기억한다. 그래서 IP 주소에 단어로 된 이름을 부여하는 것이 좋을 것.

### Domain Name

DNS(Domain name system)에 의해 resolve 될 수 있는 용어.

모든 접속을 Domain Name으로 하도록 한다면, IP 주소가 바뀌더라도 사용자는 같은 방법으로 접근할 수 있다. 관리자는 DNS 로 해당하는 IP 주소만을 변경해주면 된다.

물리적으로 가까운 곳이면 당연히 통신은 빠를 것이다. 글로벌한 서비스를 제공하려면 하나의 서버가 아니라 분산되게 서버를 운영하는 것이 좋을 것. DNS는 다른 지역에서 접근하는 서비스들을 해당 지역에 맞는, 효율적인 IP 주소로 변경해줄 수 있다.

### The Many Steps of Name Resolution

### Name resolution

도메인 이름을 IP 주소로 변경하는 과정

**DNS 없이 네트워크는 동작하지만** 컴퓨터를 사용하는 사람은 불편할 것이다. 설정해주는 것이 좋다.

총 5가지 종류의 DNS Server가 있다.
1. Caching name servers
2. Recursive name servers
3. Root name servers
4. TLD name servers
5. Authoritative name servers

### TTL (Time to live)
초 단위로 도메인 네임을 얼마나 오래 저장할지를 나타낸다. 도메인 네임의 소유자가 설정할 수 있다.
- 이전에는 트래픽을 줄이기 위해 하루 정도의 긴 값을 가졌다.
- 최근에는 몇분 혹은 몇시간 정도로 많이 설정되지만, 여전히 긴 경우도 발견할 수 있다.

### Anycast
- 위치나 네트워크 상태등을 바탕으로 트래픽을 다른 목적지로 라우팅하는 기술이다.
- 이를 이용해 하나의 IP로 전달하는 패킷이 실제로는 다른 도착지로 전달될 수 있다.

### TLD (Top-level domain)
- 도메인 이름의 마지막 부분
- `.com` 과 같은 부분
![](https://i.ibb.co/M89Qzs4/1-C6-F2-E5-A-8-B37-47-A4-AEE6-F4-E1515380-C9.png)
1. Root Server 로부터 TLD name server 의 주소를 전달 받는다.
2. TLD Name Server 로부터 authoritative name server의 주소를 전달 받는다.
3. Query
- Root Server와 TLD Name Server를 운영하는 곳은 적지만 Anycast와 같은 기술을 통해 물리적으로는 다른 주소로 트래픽이 redirect 된다.
- 이렇게 도메인 네임을 여러 단계로 얻어와야만 악의적인 공격자로 부터 안전하게 DNS 를 사용할 수 있다.

### DNS and UDP
DNS 는 TCP 대신 UDP 를 많이 쓴다. Name resolution에 드는 패킷의 차이가 많다.
![](https://i.ibb.co/N1rdnY7/34-D29208-AE1-B-435-A-92-F0-8-C563-E286-F77.png)
Name resolution의 한번의 요청과 응답의 경우, TCP connection 한번에 아래와 같은 송수신이 필요하다.

- 3 way handshake
- 요청
- 요청을 받았다는 ack
- 응답
- 응답을 받았다는 ack
- 4 way-hand shake
=> 3 + 2 + 2 + 4 = 11

![](https://i.ibb.co/3Rf0XLJ/F6560-DD6-9-ADB-4-B7-A-AD86-92-DA8-F0-E49-AC.png)
반면 UDP 의 경우 주고 받는 패킷 하나씩 2번, 총 8개의 패킷 만으로 전체 Name resolution을 처리할 수 있다. 그래서 보통 DNS는 UDP를 많이 사용한다.

## Name Resolution in Practice
![](https://i.ibb.co/30ZP03t/BBC146-EE-00-C1-4-FA8-AFC5-62-C4-C1-A8-A3-AE.png)

### Resource Record Types
DNS가 정보를 기록하는 방식은 여러가지 방법이 있다.

### A record
도메인 네임은 특정한 IPv4 주소를 가리키는데 사용된다.
- 보통은 하나의 도메인 네임은 하나의 A record를 가진다.
- 하나의 도메인 네임이 여러개의 A record를 가질 수 있는데, 이를 통해 DNS Round robin이 가능하다.

### DNS Round Robin
- 여러 IP를 반복하면서 DNS Name Resolution의 트래픽을 관리하는 방법.
- 하나의 DNS, microsoft.com에 IP 주소 4개의 A record를 가지게 했다고 가정해보자. 각각의 주소는 10.0.0.1 ~ 10.0.0.4이다. Name Resolution 요청에 DNS Resolver는 모든 IP 주소를 반환한다.
- 응답을 받은 컴퓨터는 자신이 전달받은 IP 주소 중 첫번째 주소(10.0.0.1)를 이용해야 한다는 것을 알고 있다. 그러나 첫번째 주소가 응답하지 않는 경우에 대비해서 주소 4개를 모두 가지고 있는다.
- 두번째로 microsoft.com 주소를 요청하는 컴퓨터 또한 마찬가지로 4가지 주소를 모두 전달받지만, 이번에 전달받은 주소의 첫번째 값은 10.0.0.1이 아닌 10.0.0.2 이다. 이 컴퓨터는 첫 연걸을 10.0.0.2 에 시도하게 되며, 이런 방식은 모든 DNS resolution 시도에서 동작하며, 트래픽을 분산시킨다.

### Quad A record
도메인 네임을 특정한 IPv6 주소를 가리키는데 사용한다.

### CNAME record (Canonical name record)
- 하나의 도메인에 다른 이름을 부여하는 방식
- 하나의 도메인을 다른 도메인으로 redirect 하는데 사용된다.
- 하나의 IP 주소가 여러 도메인을 가지도록 설정됬다고 해보자. 만약 IP 주소가 변경된다면 각각의 도메인의 A record를 변경하는 작업을 해야한다.
- CNAME record를 이용해 대표 도메인을 설정하고, 다른 도메인들은 CNAME record를 통해 대표 도메인을 가리키도록 설정한다면 추후에 IP 주소가 변경되더라도 하나의 A record만 변경하는 것으로 수정을 최소화 할 수 있다.

### MX record (Mail exchange)
- e-mail을 올바른 서버로 전달하기 위해 사용되는 레코드
- 많은 회사들이 웹과 메일 서버를 다른 기기, 다른 IP 주소에서 서비스하게 된다. MX record 는 웹 트래픽은 웹서버, 메일 트래픽은 메일 서버에게 전달될 수 있도록 한다.

### SRV record (Service record)
- MX record와 유사하게 여러가지 서비스들의 위치들을 알아내는데 사용된다.
- MX record와 동일하게 동작하는 대신, SRV record는 mail 서비스이외의 다른 종류의 서비스들에게 트래픽을 전달한다.

### TXT record (Text record)
- 도메인 네임에 대해 사람이 읽을 수 있는 노트를 위한 record.
- 현대의 네트워크에서는 다른 컴퓨터가 처리할 수 있는 추가적인 정보를 많이 포함하기도 한다. 형식의 제한이 없는 record라 이를 이용해서 DNS가 원래 의도하지 않았던 추가적인 정보까지 전달하는 것이 가능하다.
- 신뢰된 다른 네트워크 서비스와 관련된 설정들을 공유하는데 사용되기도 한다. 예를 들면 이메일 서비스와 관련된 설정들을 공유하기도 한다고 하는데 눈으로 보고 코딩을 해보기전까지는 어떤 역할을 하는지 정확하게 감을 잡기는 힘들 것 같다.

### Anatomy of a Domain Name
모든 도메인은 세가지로 나뉜다.
	1. subdomain (www) a.k.a hostname
	2. domain (.google)
		- TLD name server -> authoritive name server 사이의 전환에 사용
	3. top-level domain (.com)
		- ICANN 이라는 곳에서 관리한다.

- 이 세가지를 모두 갖추면 FQDN(Fully qualified domain name) 이라불림
- subdomain을 여러개 가지는것도 가능하다.
- 물론 글자수 등에 제한은 있다.

`subdomain.domain.top-level-domain` (ex `.help.acme.com`)

### DNS Zones
DNS Zone은 Domain namesapce의 고유한 부분으로 DNS Zone을 유지 및 관리를 하는 개인, 조직, 회사에 위임된 도메인 네임 스페이스다.
- 통상 DNS 서버 하나가 책임이나 권한을 가지는 영역을 말한다
- 여러 레벨의 도메인 네임을 더 다루기 쉽게 해주는 것이 목적
- zone 은 겹치지 않는다.
![](https://i.ibb.co/7G2k4z0/8-D93-F3-FB-EF8-C-4988-A125-7-B398723-B0-E4.png)

- 3개의 zone 과 하나의 대표 zone 으로 분리하면, 하나의 zone과 600개의 A record를 쓰는 것보다는 낫다.
    - largecompany.com
    - la.largecompany.com
    - pa.largecompany.com
    - sh.largecompany.com

### Root name server, Root zone

각각의 TLD 서버들은 특정 TLD의 zone들을 커버한다.

authoritatve name server 들은 더 세분화된 zone들을 커버한다.

### Zone files

특정 zone에 대한 모든 resource record를 저장하고 있는 configuration file

### SOA record (Start of authority)

zone과 해당 zone을 관리하는 name server의 이름이 저장되어 있는 record

**Zone file** 이 가지고 있다.

### NS record

이 zone을 책임지는 또 다른 name server 들을 가리키는 레코드

single point of failure 를 피하기위해 하나의 zone 을 여러 물리적 서버에서 관리할 수 있다.

### Reverse lookup zone files

IP에 해당하는 FQDN(Full Qualified Domain Name)을 가지고 있는 파일

### Pointer resource record

Resolves ip to a name.

## Dynamic Host Configuration Protocol

### Overview of DHCP

- IP
- Subnet mask
- Gateway
- Name server

네트워크 설정을 할떄, 반드시 설정해야하는 것들은 위 4가지 인데 여러 기기에 모두 설정하려면 귀찮은 일이 될 것 이다. 마지막 3개는 같은 네트워크 안이라면 보통 같을 것이다. IP는 달라져야 한다.

DHCP는 이런 귀찮은 일을 해주기 위해 필요하다.

### DHCP (Dynamic Host Configuration Protocol)

호스트에서 네트워크 설정들을 자동으로 하게 해주기 위한 application layer protocol

- 기기가 DHCP 서버에 네트웤 설정을 요청하면 네트웤 설정들을 한번에 받아올 수 있다.
- 관리자의 일을 줄여줄 뿐만 아니라 어떤 IP를 어떤 기기에 할당하는지 정하는 것도 설정할 수 있다.
- DHCP가 네트워크 관리자가 해야하는 설정들을 자동으로 해주는 것 뿐만 아니라 IP를 어떻게 할당하는가에 대한 문제도 해결해 준다.

**모든 기기의 모든 IP 들이 외부 네트워크로부터 공개될 필요는 없다. 서버 장비나 게이트웨이 라우터의 IP는 static 하고 public한 형태로 관리되어야 한다.**

예를 들면 같은 네트워크의 장비들은 게이트웨이의 주소를 항상 알고 있을 필요가 있다.

만약 로컬 DNS 서버가 동작하지 않는다고 해보자. 네트워크 관리자는 새로운 DNS 주소를 통해 문제를 해결 할 수 있을 것이다. DNS 서버를 static 하게 설정하지 않는다면, DNS 서버가 동작하지 않을때 문제점을 확인하는 것이 어려울 것이다.

그러나 다른 클라이언트 기기들은 올바른 네트워크 안에서 올바른 IP 주소를 가지고 있는가만 중요한 문제가 된다. 정확히 어떤 IP 주소를 가지느냐는 별로 중요하지 않다. DHCP를 사용하면 이러한 클라이언트 기기들을 위한 IP 주소들을 따로 할당해 둘 수 있다. 이런 방식으로 클라이언트 기기들은 필요할 때마다 IP 주소를 할당받을 수 있다. 그러면 모든 네트워크 장비들과 해당하는 IP 의 목록들을 관리할 필요가 없다.

**DHCP dynamic allocation**

A range of IP addresses is set aside for client devices and one of these IPs is issued to these devices when they request one.

**DHCP automatic allocation**

Automatic allocation is very similar to dynamic allocation, in that a range of IP addresses is set aside for assignment purposes. The main difference here is that the DHCP server is asked to keep track of which IPs it’s assigned to certain devices in the past. Using this information, the DHCP server will assign the same IP to the same machine each time, if possible.

(해석하는 것보다 원본이 더 의미를 잘 전달하는 것 같다)

**DHCP fixed allocation**

Fixed allocation requires a manually specified list of MAC address and the corresponding IPs.

- 보안상의 목적으로 이렇게 쓰일 수 있다.

NTP 서버를 할당하는 등에도 사용할 수 있다.

### DHCP in Action

DHCP는 application layer 프로토콜이지만 DHCP의 포인트는 이 자체로 네트워크 레이어의 특징들을 결정하는데 있다. 아래와 같은 과정으로 설정을 공유할 수 있다.

### DHCPDISCOVER
![](https://i.ibb.co/KNmvpw7/4-D4-D4921-4-BBF-49-CB-8-E7-A-45-DD6-A3-EAF72.png)

네트워크 설정을 얻기 위해 클라이언트의 시도

최초의 클라이언트는 항상 68포트로부터 67포트에게 브로드캐스트로 DHCPDISCOVER 메시지를 보낸다. Transport layer의 프로토콜은 UDP를 사용한다. 이런 메시지는 네트웤의 모든 노드에 전달되며 만약 DHCP 서버가 있다면 서버는 DHCPOFFER 메세지를 보낸다.

### DHCPOFFER
![](https://i.ibb.co/0tZxKrP/6-E3798-BF-CCD6-4476-9215-5-C7760249-D38.png)

서버는 DHCPOFFER 메세지를 통해 클라이언트에게 IP 주소를 전달하는데 이 메세지의 패킷은 브로드캐스트로 전달된다. 최초의 DHCPDISCOVER 를 전송한 클라이언트는 DHCPOFFER 메세지에 포함되어있는, 요청을 보낸 클라이언트의 MAC 주소로부터 해당 DHCPOFFER 메시지가 자신에게 보내진 것임을 알 수 있다.

이떄 서버가 할당하는 IP 주소는 앞에서 알아본 allocation 설정이 어떻게 되어있느냐에 따라 할당된다.

클라이언트는 이런 DHCPOFFER 를 거절할 수 있는데, 여러 DHCP 서버가 네트워크에 있는 환경에서 특정 IP 주소를 거절하는 식으로 구현되기도 한다. 그러나 드문 케이스다. 클라이언트는 DHCPOFFER 메시지의 응답으로 DHCPREQUEST 메시지를 보낸다.

### DHCPREQUEST
![](https://i.ibb.co/kJ3wdg7/A6-DAB9-A7-2-D87-47-C9-80-CF-17373-D2-B2244.png)


이 메시지는 보통 앞선 DHCPOFFER를 통해 할당된 IP 주소를 사용하겠다는 동의 의사를 전달하는데, 아직 IP의 할당이 완전하게 이루어진 것은 아니기 때문에 0.0.0.0:68 의 주소로, 브로드캐스트로 전달한다. 이후 서버는 DHCPACK 메시지로 응답한다.

### DHCPACK
![](https://i.ibb.co/6DSGcmq/D3-CD8-DF4-6-EB6-4-EB7-9055-A6-F5-C03-BC5-A0.png)

DHCPOFFER와 마찬가지로 메시지에 포함되어 있는 MAC 주소를 통해 클라이언트는 본인에게 보내는 메시지임을 확인할 수 있다. 이제 클라이언트는 이제 DHCP Server를 통해 네트워크 설정을 할 수 있다.

이 단계에서 DHCP 클라이언트 역할을하는 컴퓨터에는 연결된 네트워크에서 본격적으로 작동하는 데 필요한 모든 정보가 있어야 한다. 이런 정보를 **DHCP lease** 라고 한다.

DHCP lease 는 expire time 을 가지고 있는데, 클라이언트가 가지고있는 lease가 파기되면 DHCP Discovery 부터 시작해서 다시 DHCP의 모든 과정을 반복해야한다.

클라이언트 또한 DHCP Server에게 lease 를 생성할 수 있는데, 네트워크로부터 disconnect를 할때 사용한다. 이 정보를 통해 DHCP server는 할당되었던 IP를 다시 재할당 가능한 IP pool에 추가할 수 있다.

## Network Address Translation

### Baiscs of NAT

게이트웨이(일반적으로 라우터 또는 방화벽)가 발신 IP 데이터 그램의 소스 IP를 다시 쓰면서 원래 IP를 유지하면서 응답에 다시 쓰도록하는 기술이다.

말만 들으면 정확하게 감이 잘 오지 않는다. 아래와 같은 네트워크 구성을 예로 들자면,

![](https://i.ibb.co/1s4F7s8/E9508974-9485-4-B7-B-9-C87-0-A144-A80-F846.png)

이런 상황에서 Router가 모든 발신 패킷에 대해 NAT를 수행하도록 설정되어있다고 가정해보자. 보통의 라우터는

IP Datagram 의 TTL을 하나 줄이고, checksum을 다시 계산하고, 원본 데이터를 수정하지 않은체로 패킷을 전달한다.

![](https://i.ibb.co/vqRxRJx/0-BFEDD9-B-EEAC-4-DE0-B773-6-F2-EFFE84-A32.png)

NAT가 설정된 경우, 라우터는 소스 IP를 자신의 주소로 변경하고 패킷을 전달한다. 패킷을 받은 Computer2 는 Computer 1이 아닌 라우터가 해당 패킷을 보낸 것으로 확인하며, 응답을 Router의 IP 주소로 전달하게 될 것이다.

라우터가 전달받은 응답이 실질적으로 Computer 1을 항햔다는 것을 안다면, 다시 Destination IP를 Computer 1의 주소로 변경하고 전달함으로써 통신이 정상적으로 동작하게 끔 할 수 있다. (다음 섹션인 NAT and the Transport Layer에서 확인)

이런 과정을 통해 Computer 1의 IP 주소를 Computer 2에게 숨길 수가 있는데, 이 것을 **IP masquerading** 이라고 한다. 이를 통해 네트워크의 IP 주소를 외부로부터 숨길 수 있다. 이게 굉장히 중요한 보안 기법인데, 외부에서 클라이언트의 IP 주소를 모른다면 통신을 시도조차 할 수 없기 때문이다. 이런 네트워크를 **one-to-many NAT** 이라 한다.

### NAT and the Transport Layer

NAT이 Network Layer에서 동작하면 그 과정은 쉽게 진행될 수 있다. Transport Layer에서 동작하게 된다면 NAT가 온전하게 동작하기 위한 추가적인 과정이 필요하게 된다.

앞서 살핀것 처럼 단순히 외부로 나가는 OUTBOUND 패킷의 주소를 변경하는 일은 어렵지 않다. 외부로 부터 들어오는 INBOUND 패킷의 경우 모두 같은 주소 (라우터의 주소)를 통해 들어오게 되는데 이 패킷이 정확히 어떤 기기에 전달되어야 하는지 알아내야 한다.

### Port Preservation

클라이언트가 선택한 소스 포트가 라우터에서 사용하는 것과 동일한 포트인 기술

![](https://i.ibb.co/18Syc4c/C76218-F8-9-DF3-4777-AA6-C-B1921-BD24-BDA.png)

(그림의 Destination IP는 Source IP로 변경되어야함. 오타)

네트워크 외부와의 connection은 ephemeral port의 가능한 값 중에서 랜덤하게 선택되었었다. 클라이언트가 선택한 값을 라우터가 동일하게 사용한다. 라우터는 IP 주소만을 변경한다.

![](https://i.ibb.co/m5psZnz/EF9-D68-D6-1-B23-450-C-8663-FE20-D2-EB7-B66.png)

전달받은 응답의 port를 통해 특정 컴퓨터에게 패킷을 전달 할 수 있다. 어떤 포트가 어떤 IP에 의해 사용되었는지는 라우터가 내부적으로 table을 통해 저장해야 할 것이다.

ephemeral port의 가능한 값은 충분히 다양하지만, 다른 컴퓨터에서 같은 포토를, 비슷한 시간대에 사용할 수도 있다. 이런 경우 라우터는 사용되지 않고 있는 랜덤한 포트를 대신 설정하고 사용한다.

### Port forwarding

특정 destination port가 항상 특정 node에게 전달될 수 있도록 하는 기술

- 기술의 핵심은, public ip 주소와 포트를 private ip와 포트로 전달할 수 있다는 점이다.
- **공유기에서** 설정을 해줘야 한다.

![](https://i.ibb.co/nCPYhpT/BC11-F99-E-C128-4-E45-BDDC-A04-CAD01-BCFE.png)

port forwarding 은 IP masqurading 뿐만 아니라 더 유용하게 사용될 수 있다. 웹서버와 메일서버 각각이 다른 물리적 서버에서 동작할때, port forwarding을 통해 외부에서 서비스에 접속할때 같은 IP 주소로 port만 다르게 전달 함으로써 다른 서비스를 이용할 수 있다.

대표적으로 같은 도메인 주소(같은 IP 주소)로 여러 서버, 여러 서비스를 운영하는 것이 가능하다. 외부에서 접근할때는 public ip (라우터 주소) 와 서비스의 포트만으로 서비스에 접근이 가능하다. (웹은 80포트, 이메일은 25포트와 같은식으로)

OUTBOUND Packet의 경우 Port Preservation, INBOUND Packet의 경우 Port Forwarding를 이용함으로써 NAT를 통해 네트워크 통신이 가능하게 된다.

### NAT, Non routable address space and the limits of IPv4

IPv4 주소의 갯수는 한정되어 있고 점점 고갈되고 있다. IPv6가 현실적인 대안이지만 IPv6가 널리 쓰이기전까지, IPv4를 사용하면서도, 더 많은 기기들을 네트워크에 연결 가능하게 하려면 어떻게 해야할까?

바로 앞에서 그것이 가능한 기술을 알아보았다. NAT 를 통해서는 하나의 public IP 주소를 통해 많은 기기들을 연결 할 수 있고, 네트워크 통신이 가능하게 한다. 네트워크 내부적으로는 Non routable address space를 사용하면서 외부에 공개된 라우터 만이 routable 한 address를 사용함으로써 이것이 가능하다.

참고: [IPv4 address exhaustion](https://en.wikipedia.org/wiki/IPv4_address_exhaustion)

## VPNs and Proxies

### Virtual Private Network

외부 host가 같은 로컬 네트워크에서 동작할 수 있도록 로컬 네트워크를 확장하는 기술

네트워크 보안을 위해서는 Firewall, NAT 등의 기술등을 통해 내부 망에 연결되어 있는 기기들만이 자원에 접근이 가능하도록 구현을 하는 것이 좋다. 그럼에도 외부에서도 이런 자원을 접근할 필요가 있을 수 있는데, 그럴때 VPN을 사용한다.

![](https://i.ibb.co/dBrVfMh/651-F65-E9-12-B9-4-CAA-9-B86-8-EC2-BFA7805-C.png)

대부분의 VPN은 **Transport Layer payload** 내부에 제2의 패킷을 담는 것으로 구현된다.

VPN은 two-factor authentication 이 가능하게 한 첫번쨰 기술이다.

### Two-factor authentication

인증을 위해 username 과 패스워드 이상이 필요한 기술

보통은 소프트웨어나 하드웨어의 특수한 부분을 활용해 짧은 단기간 사용할 수 있는 토큰을 만들고는 한다.

VPN은 site와 site를 연결하는데에도 사용할 수 있다.

NAT와 같이 VPN은 기술적인 컨셉일 뿐, 구체적인 사항이 모두 정의된 기술은 아니다.

VPN의 가장 중요한 컨셉은 암호화된 커널을 이용해 실제로는 물리적으로 연결되어 있지 않은 네트워크 기기를 연결되어 있는 것처럼 사용하게 한다는 점이다.

### Proxy Services

다른 서버에 접근하기 위해 클라이언트를 대신하여 작동하는 서비스

Proxy는 보통 아래의 이유들로 사용된다.

- Anonymity
- Security
- Content filtering
- Performance

Proxy는 그자체로 추상화된 개념으로, 구체적인 구현을 의미하는 것이 아니다.

Proxy는 거의 모든 Network layer에서 존재하며 수많은 예제가 존재한다.

### Web Proxy

웹 서비스를 위한 프록시로, 다양한 역할을 수행할 수 있다.

![](https://i.ibb.co/6rPspjL/FF727-DE8-C46-F-4163-A5-FB-BBF9-CAE31-D93.png)

이전에는 Web Proxy가 클라이언트와 서버 사이에서 데이터를 전달하면서 캐싱등의 역할을 하기도 했다.

요즘에는 네트워크도 빨라졌고, 페이지나 파일에 대한 캐싱이 큰 의미가 없어져서 큰 이득은 없다.

![](https://i.ibb.co/Cw4kGnJ/DC7-C0621-E8-A4-420-C-849-E-6-A6-D282-B6-DC1.png)

클라이언트와 서버 사이에 Proxy가 웹 클라이언트가 특정 사이트, 주소로의 접근을 막는 등의 용도로 사용될 수도 있다.

### Reverse proxy

하나의 서버를 가지는 것처럼 보이지만 여러 서버를 뒤에 두고 있는 서비스

![](https://i.ibb.co/7WHmqjN/CEF30082-A5-CC-4-BC1-82-B8-72-EE1664-E26-B.png)


twitter 같이 트래픽이 많은 서비스는 하나의 웹서버로는 그런 트래픽을 감당할 수 없을 것이다. 클라이언트의 입장에서는 모두 같은 서버에 접근하는 것으로 보이지만 Proxy 서버가 앞에서 많은 트래픽을 분산 시킴으로써 서버의 부하를 나눌 수 있다. DNS Round Robin과 같이 load balancing 의 일부이다.

![](https://i.ibb.co/mJJFJJd/D9-DDFE7-D-8-E67-45-A6-A127-F8-ED4-BC14991.png)

요즘 전달되는 웹 트래픽 대부분은 암호화가 되어있다. 이런 암호화를 해제하는 것 또한 많은 컴퓨팅 파워를 먹게 되는데, 이런일을 하도록 Proxy를 두고 Application Server 들은 실제 컨텐츠를 처리하는 것에만 집중하도록 서버를 구조화 할 수도 있다.

Proxy의 핵심 개념은 하나. 클라이언트와 다른 서버 사이의 중개자 역할을 하는 모든 서버는 Proxy라고 생각하면 된다.

# QUIZ

- VPN은 대부분 transport layer에서 동작한다 (Not application layer)
- Name resolution 시 Anycast 를 통해 서버의 congestion을 줄일 수 있다.
- DNS는 사용자가 안전하게 연결 될 수 있게 한다. ?? NAT도 맞지 않나
- DNS는 53포트를 사용한다.

## Question

- DHCP는 왜 application layer에서 도는가? transport layer에서 동작해도 충분하지 않나??

Not quite. Please review the videos in the “Introduction to Network Services” module for a refresher.


# REFERENCE
[https://www.coursera.org/learn/computer-networking/home/week/4](https://www.coursera.org/learn/computer-networking/home/week/4)