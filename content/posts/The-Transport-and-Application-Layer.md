---
title: "The Transport and Application Layer"
date: 2020-04-19T18:53:44+09:00
tags: ["network"]
---



# INTRO

 이제까지 배운 Network Layer 까지의 내용으로는 네트워크 상의 하나의 노드에서 다른 노드로의 통신이 어떤 원리로 이루어지는지 확인할 수 있었다. 그러나 실제로 전달되는 정보를 사용하는 것은 네트워크 기기 안에서 동작하는 프로그램들이며, 특정 프로그램이 요청한 정보는 해당 프로그램에 전달되도록 보장되어야 한다. 크롬 브라우저가 보낸 요청을 워드 프로세스가 받으면 곤란할 것 !

 이렇게 프로그램 간의 통신은 어떻게 이루어지는지 확인하지 못했는데, 이제부터 다뤄볼 Transport Layer와 Application Layer에서 이를 확인 할 수 있다. 어떻게 보면 이것이 컴퓨터 네트워크의 최종적인 목적이라 할 수 있다. 

![transport-layer-and-application-layer](https://drive.google.com/uc?export=view&id=1llT5ISTi4MQlKb_TKerKShx9A7l1aoJk)

간단하게 Transport Layer는 네트워크 트래픽이 특정 네트워크 어플리케이션으로 도착할 수 있게 한다. Application Layer는 이 어플리케이션 간에 이해하는 방식으로 통신할 수 있게 한다. 



# CONETNS

## The Transport Layer

 앞서 언급한 기능을 위해서 Transport Layer는 multiplex와 demultiplex 를 할 수 있어야 한다. 

![multiplex-demultiplex-in-transport-layer](https://drive.google.com/uc?export=view&id=1m3u70FE-NetUFTtQSLatTbbX213wOeNW)

multiplex 는 여러 프로세스가 하나의 ip 주소를 이용해 패킷을 외부로 보낼 수 있게 하는 기능이고, demultiplex 는 반대로, 하나의 입력을 여러 프로세스에 적절하게 분류해서 전달하는 기능이다. 이런 기능을 Port 를 통해 구현할 수 있다.



### Port

네트워크 컴퓨터의 트래픽의 주소를 결정하는 16bit 숫자.

- 이를 통해 하나의 네트워크 기기에서 여러 서비스를 운영할 수 있다. 
- 80번 포트로는 HTTP, 21번 포트로는 FTP, 22 포트로는 SSH 등 .. 



### Dissection of a TCP Segment 

#### TCP Segment

Ethernet payload 에 IP Datagram 이 포함되는 것처럼, ID Datagram 의 payload 에 TCP Segment 가 포함된다.  이도 마찬가지로 header와 payload 를 가진다. 

![tcp-segment](https://drive.google.com/uc?export=view&id=1MA0fdFThEyAmKE5biDN-Kh__YVr5lyLM)

#### Destination Port / Source Port

이름 그대로의 의미를 가진다. Source Port를 알아야 응답을 하는 쪽에서 어떤 어플리케이션에 응답을 전송해야하는지 알 수 있을 것이다. 



#### Sequence number

TCP Segment가 어떤 순서로 전송되었는지를 나타내는 필드



#### Acknowledgement number

다음에 올 Sequence를 나타내는 필드



#### Header Length

payload가 어디있는지를 가리키는 오프셋 필드



#### Control flags

- URG (Urgent)
  - 이 flag가 1이면 이 segment가 다른 것 보다 더 중요하다는 것을 의미하며, 자세한 내용은 이후의 Urgent pointer field 에서 확인할 수 있다

- ACK (Acknowledged)
  - acknowledgement number field 가 검사되어야 함을 의미하는 flag
- PSH (Push)
  - buffered data를 보내주기를 요청하는 flag
- RST (Reset)
  - 데이터의 송신을 실패했음을 알리는 flag
  - 못알아듣겠으니까 다시한번만 전송해줘 
- SYN (Synchronize)
  - 처음 TCP 연결을 생성할때 사용되며 sequence number field 를 받는쪽에서 확인하도록 요청하는 flag
- FIN (Finish)
  - 다 보냈으니 이제 통신을 끊자고 요청하는 flag



#### TCP Windows

Acknowledgement 필드의 값을 읽기 전에 필요한 sequence number의 범위 



#### Checksum

IP Datagram 의 checksum과 같은 역할



#### Urgent pointer field

TCP Control flag 와 함께 특정 segment가 다른것 보다 중요하다는 것을 알리는 필드.

최근에는 잘 사용되지는 않는 field 다.



#### Options 

복잡한 흐름의 프로토콜에서 사용되는 필드. 최근의 네트워크에서는 잘 사용하지 않는다.



#### Pading

Data payload 가 의도한 offset에서 시작하도록 조절하는 0의 나열.



### TCP Control Flag and 3-way-handshaking

#### 3-way-handshake

![3-way-handshaking](https://drive.google.com/uc?export=view&id=1MUKO_9lv7zhXI7W2kWoKbFs4muMjn-dk)

두 기기가 서로 같은 Protocol을 통해 통신하고 있고, 서로의 메시지를 이해함을 확인하는 과정.

- 이 후 양쪽 모두 서로에게 메시지를 전달할 수 있으며 이에 대한 응답을 받을 수 있다. 
- 보내는 쪽은 받는 쪽이 ACK 필드와 함께 응답을 보내기 때문에 어떤 메세지를 제대로 전송받았는가 역시 확인할 수 있다. 
- 통신을 끝내기 위해선 FIN flag를 전달하고, 이를 ACK flag로 다시 확인하면서 통신의 종료를 확인한다. 
- 통신을 종료하는 과정은 **4-way-handshaking** 이라고도 한다.



#### Socket 

TCP Connection의 end-point

- 실제로 Socket을 초기화(혹은 인스턴스화?) 하는 프로그램이 필요하다. 
- 어떤 포트로든 TCP packet을 전달할 수 있지만, 받는쪽에서 socket을 열어두었어야만 응답을 받을 수 있을 것이다.



#### TCK Socket States

이 부분은 번역을 하면 괜히 의미를 해치는 것 같아 원문 그대로 옮긴다. 

#### LISTEN 

- A TCP socket is ready and listening for imcoming connections
- Server-side only



#### SYN_SENT

- A synchronization request has been sent
- But the connection hasn't been established yet
- Client-side only



#### SYN_RECEIVED

- A socket privously in a listener state, has received a synchronization request and sent a **SYN_ACK** back
- But hasn't received SYN_ACK back.
- Server-side only



#### ESTABLISHED

- This means that the TCP connection is in working order, and both sides are free to send each other data.

- Both side



#### FIN_WAIT

- This means that a FIN has been sent, but the corresponding ACK from the other end hasn't been received yet.
- Both side



#### CLOSE_WAIT

- This means that the connection has been closed at the TCP layer, but that the application that opened 

  the socket hasn't released its hold on the socket yet.

- Both side



#### CLOSED

- This means that the connection has been fully terminated, and that no further communication is possible.
- Both side



 TCP를 통해 통신을 하기위해서는 양쪽다 규칙을 지켜야하지만 소켓이 정확하게 어떤 상태에 있는가는 TCP의 스펙에서 벗어난 내용이다. 곧 운영체제 마다 정의가 미묘하게 다를 수 있다. 프로그래머가 구현해서 사용할 법한 상태는 크게 `OPEN` `LISTEN` `CONNECT` `CLOSE` 정도 였던 것으로 기억한다. 



### Connection-oriented and Connectionless Protocols

#### Connection-oriented protocol

Establishes a connection, and uses this to ensure that all data has been properly transmitted



 이전의 Ethernet frame이나 IP Datagram 의 경우 checksum을 통해 전달받은 데이터가 유효한지 확인은 하지만 만약 전달된 패킷의 checksum이 일치하지 않는 경우 단순히 패기를 할뿐 재전송을 요청하거나 하지는 않는다. 이런 역할을 Transport layer에서 해주게 된다.



 TCP의 프로토콜상 전달받은 Sequence 에 대해서는 ACK field를 통해 전달받았음을 송신자에게 전달함으로써 어떤 정보를 받았고 못받았는지를 알 수 있다. 전달받지 못한 정보에 대해서는 재전송을 하며, 메시지의 순서가 도착된 시간상의 순서와 일치하지 않더라도 받는 쪽에서는 Sequence를 확인하고 올바른 순서로 메세지를 재조합 할 수 있다.



 이런 추가적인 과정은 분명 많은 컴퓨팅 파워를 사용할 수 밖에 없으며 실제 전달하고자 하는 내용보다 더 많은 정도의 트래픽을 사용할 수 밖에 없다. 모든 패킷들이 반드시 전달되어야 하는 경우에는 이런 방식의 프로토콜이 최선일 테지만, 항상 그런 것은 아니다. 일부 패킷의 손실을 감수하고서라도 빠른 전송을 위해서는 UDP 프로토콜이 사용된다.



#### Connectionless Protocol - UDP

- TCP 와 달리 connection에 의존하지 않고 acknowledgement 와 같은 개념을 지원하지 않는다.
- 일부 프래임이 유실되어도 큰 무리가 없는 영상 스트리밍의 경우 UDP가 더 효율적인 프로토콜이 된다. 



### List of TCP and UDP port numbers

- Port는 16비트 숫자로 0-65535의 값을 가질 수 있는데 이를 분리해서 사용한다. 
- 0은 네트워크 트래픽에서는 사용되지 않지만, IPC 에서는 사용될 수 있다.
- 1-1023은 **system port** 혹은 **well-known port** 로 주요하게 알려진 네트워크 서비스를 위한 포트들이다. 
- 1024-49151은 registered port로 아주 보편적이지만은 않은 서비스들 위한 포트. 
  - 예로 3306은 많은 데이터베이스 어플리케이션들이 사용하는 포트다.
- 49152-65535는 private port 혹은 ephemeral port라 한다. 외부 네트워크와의 연결을 위해 [IANA](https://www.iana.org/) 에서 사용하지 않는 것을 권고하지만 모든 운영체제가 이를 따르는 것은 아니다.
  - [Service Name and Transport Protocol Port Number Registry](https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml)
  - [List of TCP and UDP port numbers - Wikipedia](https://en.wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers)



### Firewalls

A device that blocks traffic that meets certain criteria 

- 네트워크 보안에 중요한 역할을 하는 장비다.
- 여러 layer에서 동작할 수 있지만 보통은 Transport Layer에서 특정 포트의 연결을 차단하는 식으로 많이 사용된다. 
- Firewall 자체가 네트워크 기기일 수도 있지만 현대 운영체제들은 대부분 이를 지원한다.
- 가정에서 사용되는 네트워크의 경우 보통 router 들이 firewall 의 역할도 같이 하게된다. 



![firewall](https://drive.google.com/uc?export=view&id=1PxNBj0okQkHFQQj4x-fNsLDk2HmDiNH_)



## The Application Layer 

 TCP Section도 마찬가지로 data payload를 가지고 있다. Application Layer 의 패킷들은 마찬가지로 다시 TCP Section 의 data payload에 포함되게 된다. 

![transport-layer-and-application-layer](https://drive.google.com/uc?export=view&id=1llT5ISTi4MQlKb_TKerKShx9A7l1aoJk)

 그런데 이제까지의 Layer들은 대표적인 프로토콜이 몇가지 있었지만, Application Layer에서는 Application 마다 사용하는 프로토콜이 재각각이기 때문에 대표적인 프로토콜이라고 할만한 것이 없다. 대신 Application의 종류에 따라 공통적으로 사용되는 프로토콜은 있다. 대표적인 것이 웹 브라우저와 웹 서버가 사용하는 HTTP 이다. 다양한 모든 브라우저들이 사용하는 프로토콜은 HTTP로 모두 같다. 



### The Application Layer and the OSI Model 

![osi-layer-model-and-5-layer-model](https://drive.google.com/uc?export=view&id=1FuMAPAR9yhM1k3ElrTJTgl1AXtWC_5Uo)

 

 이제까지 5 Layer Model 로 네트워크 구조에 대해 공부해 봤는데 일반적으로 많이 쓰이는 모델은 OSI 7 Layer Model 도 많이 사용된다. 



#### Session Layer

- Application Layer 와 Transport Layer 사이의 통신을 가능하게 한다.
- TCP Segment로 부터 Application Layer의 데이터를 추출하고 이를 Presentation Layer에 전달한다.



#### Presentation Layer

- Session Layer로 부터 전달받은 Application Layer의 데이터가 application이 이해할 수 있도록 하는 역할을 한다.



Session Layer와 Presentation Layer는 운영체제의 일부로 새로운 단계의 캡슐화가 이루어지는 것은 아니다. 그래서 두 Layer를 Application Layer와 같이 이해해도 큰 무리는 없다. 



### All the Layers working in Unison 

![tcp-example](https://drive.google.com/uc?export=view&id=15xgCzhAQwWIuY_jQa-jqsvotl-mM8x_E)



 이제까지의 과정을 한번에 살펴보자. 컴퓨터 1이 브라우저를 통해 웹 서버 컴퓨터 2에 웹 페이지를 요청하는 과정이다. 

1. 컴퓨터 1은 요청하고 있는 IP 주소가 자신의 네트워크에 없는 것을 확인한다.

2. 다른 네트워크로 요청을 전달하기 위해 자신의 네트워크 안의 라우터를 찾아 내용을 전달한다.

   - ARP Broadcast 를 통해 라우터의 MAC 주소를 얻는다.
   - Source Port, Destinaion Port 등의 정보로 TCP Segment를 완성한다.
     - set SYN flag
     - calculate checksum
   - Source IP, Destination IP 등의 정보로 IP Datagram을 완성한다. 
     - set TTL = 64 (보통 많이 사용하는 값)

   - Ethernet frame을 완성시키고, 라우터에 전달한다.

3. 라우터 A는 전달받은 패킷을 다시 라우터 B에게 전달한다.
   - 주소 확인
   - checksum 확인
   - Ethernet frame을 제거
   - IP Datagrame 의 TTL을 하나 감소시키고 다시 체크섬을 계산
   - Ethernet frame을 다시 구성 (라우터 B의 MAC Address를 사용)
4. 라우터 B는 같은 과정을 반복해 컴퓨터 2에게 패킷을 전달한다. 
   - IP Datagram의 IP 주소를 ARP 테이블에서 확인함으로써 MAC 주소를 확인할 수 있다. 
5. 컴퓨터2는 전달받은 패킷을 검증하고 이에 맞는 TCP response를 보낸다.



### Failed Quiz

![image-20200419232335995](https://drive.google.com/uc?export=view&id=1sPvzcEVXwNFaPzYMUGBJvRwSV31LIKCN)

Multiplexing 이 맞다. 영어 해석을 잘 못한거같음.. 



![image-20200419232511129](https://drive.google.com/uc?export=view&id=1H1kD5QPfzMVnzEv2H6XYbFqIDg39ytZt)

이거는 SYN 플래그가 맞다. 



# REFERENCE

[The Bits and Bytes of Computer Networking](https://www.coursera.org/learn/computer-networking/home/welcome)