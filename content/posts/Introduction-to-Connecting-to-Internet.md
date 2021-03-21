---
title: "Introduction to Connecting to Internet"
date: 2020-07-02T22:04:43+09:00
tags : ["network"]
---



# INTRO

  이 강의를 사실 한달만에 다 들으려고 했는데 실패했다. 그래서 한달치를 더 결제 했어야 했는데 뭔가 아쉬워서 청강으로 마무리를 할까 하다보니 지지부진, 계속 미루게 되더라. 네트워크 말고도 데이터베이스, 알고리즘, 운영체제등 다시 기초를 잘 다지고 싶은 분야들이 많은데 이거 먼저 끝내놔야 다음 강의를 잘 들을 수 있을 것 같다는 생각이 들었다. 



 이번에 네트워크 강의 노트를 정리하다가 알게된 몇가지 사실이 있는데, 억지로 번역하려고 하지말고, 모든 정보를 담으려고 하지 말자. 그냥 핵심만 정리해두고, 나중에 필요해졌을때 찾아서 볼 수 있을 정도면 충분한 것 같다. 내가 정리한 것보다 더 잘 정리된 자료도 잘 찾아보면 있을 수도 있다. 강의 내용을 손으로 옮기려는 노력보다 우선 강의를 잘 듣고 잘 받아들이는 게 먼저 인 것 같다. 노트는 그다음이다.



 다음에 데이터 베이스, 운영체제들도 유사하게 정리해가면서 공부해 갈 것 같은데 다음번에는 노트 정리에 너무 큰 신경을 쓰지 않도록, 강의 내용에 집중하도록 해야겠다. 



# CONETNS

## POTS and Dial-up

### Dial-up, Modems and Point-to-Point Protocols 

#### PSTN (Public Switch Telephone Network)

a.k.a. POTS(Plain Old Telephone Service)



1970년대에는 이미 전화망이 잘 깔려있었는데, 컴퓨터 통신은 원할하지 못했다. 전화망을 쓰면 멀리 있는 컴퓨터간의 통신도 잘 할 수 있을 것이라 생각했다.



#### Dial-up

- POTS를 연결하는데 사용했다. 
- 연결은 실제로 전화번호로 전화를 하는 것과 같은 원리였다.



#### Modem 

![modem](https://drive.google.com/uc?export=view&id=1lOtUFa3ugoZl404KerDTQnO3UNvxREyH)

- **Mo**dulator / **Dem**odulator
- 전화로 음성을 전달하는 것과 01의 바이트 스트림을 전달하는 것은 비슷한 원리로 가능했다.



#### Braud rate

- A measurement of how many bits can be passed across a phone line in a second
- 처음엔 별로였는데 점점 발전하기 시작했다.



## Broadband Connections

### What is broadband?

- Any connectivity technology that isn't dial-up internet
- Almost Always faster than dial-up 



dial-up 의 방법과는 달리 연결이라는 것이 필요없다. 기본적으로 항상 연결되어있는 상태를 유지한다. 



### T-Carrier Technologies

- Originally invented by AT&T in order to transmit mulitple phone calls over a single link
- 이걸 데이터를 전송하는데 활용할 수 있게 개발했다. 데이터 전송속도가 크게 올라갔다.
- 이걸 T1 (First T-Carrier specification)이라하는데, 이 T1 여러개를 또 하나의 링크에서 사용하는 방법을 개발하는 등으로 발전해나갔다. 



### Digital Subscriber

- Digital Subscriber Line 이라는 기술로 전화 회사들의 기존 인프라 구조를 유지하면서 더 나은 데이터 전달을 할 수 있게 되었다. 
- 하나의 라인에서 전화와 데이터 통신을 동시에 할 수 있었다. 
- ADSL (Asymetric DSL)
  - Faster download speed, slower upload speeds
- SDSL (Symetric DSL)
  - upload and download speed is same
  - used by server
- HDSL 
  - SDSL 보다 더 빠름



### Cable Broadband

유선 통신이 전화기에 기반했다면, 무선통신은 텔레비전 기술에 기반한다.

텔레비전의 주파수를 방해하지 않는 범위에서 데이터를 전송할 수 있었다.

Cable이 Shared bandwidth technology 라는 것이 주로 달랐다. 



### Fiber Connections 

- 전기적 신호가 아닌 빛을 사용한다.
- 다른 연결보다 더 멀리, 손실 없이 갈 수 있다. 



#### FTTX

- Fiber to the x
- FTTN - Fiber to the neighborhood
- FTTB - Fiber to the building 
- FTTH - Fiber to the home
- FTTP - FTTH and FTTB



#### Optical Network Terminator

- FTTP의 demarcation point는 modem이 아니라 Optical Network Terminator이다. 

- ONT는 fiber network의 신호를 전통적인 twistted pair copper network가 이해할 수 있게 변환한다. 



## WANs

### Wide Area Network Technologies

- Acts like a single network, but spans across multiple physical locations

![wan](https://drive.google.com/uc?export=view&id=16BUoXMqR0rKrW2lcjCSI8XRlbA0cwGOU)

- ISP를 통해 사용할 수 있다. 두 지역간의 통신을 ISP가 해줄 것이다.
- data link layer 에서 여러 protocol을 사용한다. 



#### local loop

- The area between a demarcation point and the ISP's core network 



### Point-to-Point VPNs

![point-to-point-VPN](https://drive.google.com/uc?export=view&id=1MmpdT1oxq0OUScKGSu_0INx84uqeLnx7)

- 한 유저를 대상으로 VPN을 제공하는 것과 비슷한 원리로 동작한다. 





## Wireless Networking 



### Introduction to Wirless Networking 



#### WiFi

- IEEE 802.11 standard a.k.a WiFi
- Radiowave를 통해 통신한다 
- Wifi는 보통 간단한 프로토콜을 공유하지만 Frequncy band는 다를 수 있다.



![802.11](https://drive.google.com/uc?export=view&id=1P6yElZ4DlPLmQ4mifs0mbvW4AZXPVB7y)

- 802.11 = physical and data link layers



#### frame control fields

- version 



#### Duration 

- how long the total frame is 



#### Address

- 왜 4개의 주소일까? 2개가 아니라 
- 어떤 access point가 프레임을 처리해야할지 가리켜야 할 필요가 있기 때문에
- source address field
  - MAC of sending device
- destination address field
  - Intended destination point on the network 
- receiving address
  - MAC of the access point that should recevice the frame
- transmitter address
  - MAC of whatever has just transmitted the frame
- 보통은 source address == transmitter address && destination == receiving 



#### Sequence Control

- Sequence number used to keep track of ordering the frames



#### FCS

- Frame checksum 



대부분의 경우에 destination과 



#### Wireless access point 

A device that bridges the wireless and wired portions of a network



### Wireless Network Configurations

크게 3가지 종류가 있다. 

- Ad-hoc networks

- Wireless LANS (WLANs)

- Mesh Network 



#### Ad-hoc network

In an ad-hoc network, there isn't really any supporting network infrastructure. Every device involved with the network communicates with every other device within range, and all nodes help pass along messages. 



#### WLANs

![WLAns](https://drive.google.com/uc?export=view&id=12DhB8vvMUbBMM12yBu-cg7Ic8wCqU38z)

- LAN과 같이 모든 연결이 Access point를 거쳐 외부로 나가는 구조 
- 내가 와이파이를 집에서 쓰는것도 이런 구조이겠구나 



### Wireless Channels

Switch를 통해서 물리적인 인터페이스와 기기를 기억함으로써 collision domain을 해결할 수 있었다. 

Wirless network 에서는 물리적인 연결이 없는데 어떻게 collision domain을 피할 수 있을까?

라디오는 80Mhz ~ 108Mhz 등의 주파수를 쓴다.

Wireless network은 2.4Ghz ~ 5Ghz 의 주파수를 사용한다. 

![channels](https://drive.google.com/uc?export=view&id=1dW9eLBhlEMzzYkPG82xecE-Alwx8kQ4W)

- 2.4Ghz 를 사용하는 것은 2.4Ghz ~ 2.5Ghz 사이의 주파수를 사용한다는 것을 의미한다. 
- 채널마다 사용하는 주파수가 다르다.
- radio wave는 오차가 있을 수 있기 때문에 정확한 주파수 근처에 버퍼같은게 필요하다
- 어떤 채널은 겹치고, 어떤 채널은 겹치지 않는다.
- 22Mhz 의 주파수 범위에서 1, 6, 11 채널만이 전혀 겹치지 않는다.
- 오늘날의 네트워크 장비들은 어떤 채널이 가장 바쁜지도 감지한다. 
- 그럼에도 불구하고 한 채널에 congestion이 존재할 수도 있다. 



### Wireless Security

이론적으로 모든 통신을 가로챌 수 있다.



#### WEP

 it's an encryption technology that provides a very low level of privacy.

- key size가 40bit 밖에 되지 않는다. 
- 뚫는데 몇분이면 가능할 것



#### WPA

- WEP을 개선한 버전
- 128bit key size



#### WPA2

- 256bit key size



#### MAC filtering 

With MAC filtering, you configure your access points to only allow for connections from a specific set of MAC addresses belonging to devices you trust.



### Cellular Networks 

a.k.a mobile networking 

- 802.11 이 여러 구현이 있듯 여러 구현이 있다.

- WiFi 처럼 radio wave를 통한 통신을 하지만 더 멀리 통신할 수 있다. 





## QUIZ 

Cellular network towers are configured in such a way so that they avoid what type of problem?

- Overlap 

Which of the following is NOT a Wide Area Network (WAN) connection type?

- ATM x
- Frame relay x

Which configuration is considered to be a common way to increase security in a wireless network?

- WEP

A point-to-point virtual private network (VPN) utilizes this type of device at each point.

- firwall

Modems communicate data by using which method?

- Audiable wavelengths





# REFERENCE