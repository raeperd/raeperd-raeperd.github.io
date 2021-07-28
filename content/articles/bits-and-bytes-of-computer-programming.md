---
title: "The Bits and Bytes of Computer Networking"
date: 2020-07-16
main: true
cover:
  image: https://i.ibb.co/hMRY96C/the-bits-and-bytes-of-computer-networking.png
---

# Motivation

 매번 네트워크 공부는 해야지 해야지 하면서 미루게 된다. 최근에 소켓 통신 프로그래밍을 하게 될 기회가 생겼는데 이때서야 그제껏 네트워크 공부를 미뤄왔던 내 자신을 반성해본다. 네트워크 뿐만 아니라 다른 과목도 마찬가지다. 다 알면 좋은데 귀찮은 것들 투성이다.
밑바닥부터 시작해 네트워크의 모든 부분을 이해하고 넘어가면 참 좋을 것 같지만 그만큼 만만한 과목이 아닌 것도 알고, 그정도의 전문성을 가지고 싶지는 않았다. 

학부때 꼭 네트워크 수업을 들어보고 나왔어야 했다. 중요한 분야라는 건 그 당시에도 알고있었고 심지어 수강신청도 했지만 첫 수업만 듣고 나와버렸다. 중간고사, 기말고사 기간에 ppt 만 달달 외우게 할 수업인게 너무 뻔히 보여서.. 솔직히 말해서 첫수업만에 교수님의 무능이 너무 뻔히 보였다. 

# About Course
[The Bits and Bytes of Computer Networking - Coursera](https://www.coursera.org/learn/computer-networking?)

어디가서 네트워크 잘 모른다는 소리만 안듣게 공부를 시작하려고 했는데, 잘 정리된 네트워크 강의가 생각보다 잘 없다. 프로그래밍 언어에 대한 입문 강의는 참 많은데 네트워크에 대해서는 사람들이 많이 궁금해 하지는 않는 것 같다. coursera 에서 구글이 제공해주는 강의가 있는 것을 확인했고 이걸 끝까지 완주해보기로 했다. 이전에 머신러닝 강의를 너무 잘 들어서 coursera 강의면 일단 신뢰가 잘 간다. 코딩으로 테스트를 보는 과정까지 있었으면 좋았겠지만 아쉽게도 그런 과정은 없는 것으로 보인다. 

## Note 
1. [TCP/IP 5Layer Model](../notes/network/tcp-ip-5-layer-model)
2. [The Basics of Networking Devices](../notes/network/the-basics-of-networking-devices)
3. [The Data Link Layer](../notes/network/the-data-link-layer)
4. [The Network Layer](../notes/network/the-network-layer) 
5. [IP](../notes/network/internet-protocol)
6. [Subnetting](../notes/network/subnetting)
7. [The Transport and Application Layer](../notes/network/the-transport-and-application-layer)
8. [Network Services](../notes/network/network-services)
9. [Introduction to Troubleshooting and the Future of the Networking](../notes/network/introduction-to-troubleshooting-and-the-future-of-the-networking)



# 후기
![](https://thenode.biologists.com/wp-content/uploads/2019/12/Node-Network-Graphic1-e1575627315754.gif)

우리가 매일같이 사용하는 스마트폰, 컴퓨터 모두 네트워크가 연결되었다. 당연하다는 듯이 요금만 내면 아무리 먼거리에 있는 사람과도 통화를 하고 카카오톡으로 메세지를 주고 받을 수 있고, 직접 방송을 송출하는 사람의 영상을 볼 수도 있으며, 여러 서버에 내 아이디와 패스워드, 쇼핑리스트 같은 내용을 저장해두고 필요할때마다 찾아본다. 당연하다고 하면 당연한 거지만 도대체 왜 그런게 가능할까?

단 한번이라도 의문을 가져보면 그때부터는 모든게 다 미스테리에 빠진다. 유튜브는 어떻게 그 많고 많은 스마트폰중에 지금 동영상을 클릭한 스마트폰이 바로 나의 스마트폰인걸 알고 그걸 또 어떻게 무선으로 전달하지? 내 폰에는 아무것도 연결이 안되어있는데 ..

사람과 사람이 대화하는 것을 먼저 생각해보자. 누군가와 대화를 나눌때 우리는 보통 인사를 먼저 주고 받기도 하고, 아주 친한 사이인 경우 그냥 다짜고짜 본론으로 들어가기도 한다. 한 사람한테만 말하는 것이 아니라 여러 사람한테 말하는 경우도 있을 것이고, 방금 전의 대화를 잘 못들었다면 다시 말해달라고 요청하기도 한다. 다른 사람이 이야기를 하는 동안에는 내 이야기를 하기보다는, 그사람의 말을 끝까지 듣고 난뒤 그에 대한 내 생각을 말한다. 알게 모르게 우리는 서로 잘 소통하기 위해 암묵적인 약속을 지키고 있다.
컴퓨터나 다른 네트워크 기기들도 마찬가지다. 서로 잘 소통을 하기 위해서는 어떤 “약속”이 필요하다. 한번쯤 들어본 프로토콜이 이런 역할을 한다.

![](https://premierbiotech.com/innovation/wp-content/uploads/2017/03/Testing-Protocols.jpg)

### Protocol
- 컴퓨터끼리 서로 잘 소통하기 위해 지켜지는 표준들의 집합
* 서로 대화하기 위한 약속

### Computer Networking
* 컴퓨터끼리 소통하는 방법에 대한 통칭

네트워크를 공부한다는 건 단지 이 두가지가 어떻게 이루어지는지에 대한 디테일에 대해 공부하는 것이다. 라이브러리를 가져와 프로그래밍을 하는 것처럼, 개발자들은 알게 모르게 이런 약속들을 가져와서 사용하고 있는 것이다. 이 약속의 구현체는 운영체제가 되기도 하고, 프로그래밍 언어가 되기도 하고, 제 3의 라이브러리가 되기도 한다. 지루하고 따분한 이야기가 되기 쉽지만, 큰 맥락을 이해하는 것만으로도 매일의 프로그래밍에 실질적인 도움을 얻을 수 있다. 완주 해볼만한 가치가 있는 강의였다. 
