---
title: "TCP/IP 5 Layer Model"
date: 2020-05-12
tags: ["network"]
---

보통 OSI Seven-Layer 모델과 함께 가장 많이 언급되는 모델이다. 여기서 모델이라 함은, 컴퓨터 네트워크가 어떻게 구성되어있는지를 설명하는 방법에 지나지 않는다. TCP/IP Five-Layer 모델과 OSI Seven-Layer 모델은 같은 대상을 조금 다르게 설명할 뿐 하고자 하는 일은 같다. 강의에서 TCP/IP Five-Layer 모델을 주로 하기 때문에 앞으로도 이 내용으로 정리하고자 한다. 

￼
![](https://i.ibb.co/18VGfgY/5-layer-model.jpg)

##  Physical Layer
* 케이블의 연결이나 전원 같은 물리적으로 데이터를 전송하는 계층

## Data Link Layer
* a.k.a. **Network Interface Layer** or **Network Access Layer** 
* Physical layer 의 기능들을 공통의 인터페이스로 추상화하는 단계
* 서로 다른 네트워크 기기들이 신호를 해석할 수 있는 약속, 프로토콜이 필요한 첫번째 Layer다.
* 많은 프로토콜이 있지만 **Ethernet** 과 **Wifi** 가 가장 보편적인 프로토콜이다.

## Network Layer
![](https://i.ibb.co/N2DFMF2/internet-network-layer.jpg)
- a.k.a. **Internet Layer**
* 서로 다른 네트워크가 라우터를 통해 통신할 수 있도록 한다.
* 라우터를 통해 같이 연결된 네트워크의 공동체를 **인터넷**이라고 한다.
* Data Link Layer 와의 차이점은 Data Link Layer 는 하나의 Link 에서 데이터를 주고 받지만 Network Layer 에서는 여러 네트워크를 거쳐 데이터를 주고 받는다는 차이가 있다.
* 여기서 가장 많이 쓰이는 프로토콜이 **IP** (Internet Protocol) 이다.

## Transport Layer
* **구체적으로 어떤 프로세스가 데이터를 가져가야 하는지 결정해준다.**
* **TCP** 와 **UDP** 가 가장 대표적인 프로토콜이다. 대표적인 차이점은 UDP 는 데이터의 무결성을 보장하지 않는다는 것

## Application Layer
* 웹 브라우저와 이메일 프로그램과 같이 어플리케이션 마다 다른 약속(프로토콜)을 가질 수 있다.
* **http**, **smtp** 와 같은 프로토콜이 있다.
직관적으로 아래의 그림과 같은 역할을 한다.

## Example
![](https://i.ibb.co/pLgVGmq/layer-by-example.jpg)
- The physical layer is the delivery truck and the roads. 
- The data link layer is how the delivery trucks get from one intersection to the next over and over. 
- The network layer identifies which roads need to be taken to get from address A to address B. 
- The transport layer ensures that delivery driver knows how to knock on your door to tell you your package has arrived. 
- And the application layer is the contents of the package itself.

## Reference
[The TCP/IP Five-Layer Network Model - Introduction to Networking | Coursera](https://www.coursera.org/learn/computer-networking/lecture/BTLgy/the-tcp-ip-five-layer-network-model)  
[OSI model Review](https://www.sans.org/reading-room/whitepapers/standards/osi-model-overview-543)  
[OSI model - Wikipedia](https://en.wikipedia.org/wiki/OSI_model)
