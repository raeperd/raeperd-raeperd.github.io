---
title: "Introduction to Troubleshooting and the Future of Networking"
date: 2020-07-16T21:14:14+09:00
tags: ["network"]
---



# INTRO

 네트워크의 많은 레이어에서 에러를 감지하고 수정하려는 노력을 하지만, 여전히 네트워크 환경에서 오류는 빈번하게 일어난다. 이런 오류들을 발견했을때 어떻게 troble shooting을 잘 하는가도 개발자로서 중요한 역량이 될 것이다. 



# CONETNS

## Verifying Connectivity

### Ping: Internet Control Message Protocol

#### ICMP(Internet Control Message Protocol)

- TCP/IP에서 IP 패킷을 처리할 때 발생되는 문제를 알리거나, 진단 등과 같이 IP 계층에서 필요한 기타 기능들을 수행하기위해 사용되는 프로토콜
- IP와 하나의 쌍을 이루면서 동작한다.
- Data section에는 IP  header와 Data payload 의 첫 8바이트가 저장되어있다. 



![icmp-packet](https://drive.google.com/uc?export=view&id=1Q29W1kVQUTmI05kkeS08Xlie_U9CtAlE)





#### Ping

Ping lets you send a special type of ICMP message called an **Echo Request**

- 운영체제에서 대부분 지원한다.
- 특정 플래그를 주면 다르게 동작하게 할 수도 있다. 





![ping-powershell](https://drive.google.com/uc?export=view&id=1yT7I6lbjZqTXvVMMmAdxycQx6pDzP5TR)



![ping-ubuntu](https://drive.google.com/uc?export=view&id=1PgdHug5G0ZIflyoC1RbdxuYwwLTrjSPu)



### Traceroute 

an awesome utility that lets you discover the paths between two nodes, and gives you information about each hop along the way.



- ping 은 특정 주소가 접근 가능한지 확인이 가능하다.
- 연결 까지의 많은 라우터들 중 어떤 라우터가 문제의 원인인지 알기 위해서는 Traceroute를 사용할 수 있다. 
- TTL field가 0이 되면 ICMP Time Exceeded message가 원래 주소로 전달된다. 
- Traceroute는 첫 패킷은 TTL을 1로, 두번째는 TTL을 2로 하는 방법을 사용해, 어떤 라우터가 문제를 일으키는지 확인할 수 있다. 
- `traceroute` on linux and Mac, `tracert` on Windows
- 시간에 걸친 결과 변화를 확인하기 위해서는 `mtr` (linux, mac) 나 `pathping` (windows)를 사용할 수 있다. 



### Testing Port Connectivity

transport layer에서 오류가 발생한다면 ?



#### netcat 

```shell
$ nc google.com 80
$ nc -z -v google.com 80
```



#### Test-NetConnection

```powershell
$ Test-NetConnection google.com # ICMP Ping
$ Test-NetConnection -port 80 google.com 
```



## Digging into DNS

### Name Resolution Tools 

#### nslookup

```shell
$ nslookup twitter.com
```

- nslookup 만 사용하면 interactive mode 
- 도메인 내임을 입력하면 ip 주소를 얻을 수 있다.
- server 명령으로 ip 주소를 지정할 수 있다.
- set type=MX 와 같은 명령으로 레코드 타입을 지정할 수도 있다. 
- set debug 모든 패킷을 확인 할 수 있다. 



 ![nslookup](https://drive.google.com/uc?export=view&id=109c2fR31NBewoimN1Q_7QWpd0-BIhfOV)



### Public DNS Servers 

#### Public DNS Servers 

Name servers specifically set up so that anyone can use them for free

- public DNS 서버를 통해 네트워크 연결 여부를 판단하면 편하다.

- ping 8.8.8.8을 이래서 해보는 거구나 !
- 대부분의 public DNS server 들은 anycast 를 이용해 어디서든 사용할 수있다.
- DNS server를 조작해서 악의적인 행동을 하는 것이 대표적이다. DNS server 주소를 조심해야한다. 



#### Level 3 

One of ISP provide public dns severs like 

- 4.2.2.1, 4.2.2.2, 4.2.2.3, 4.2.2.4, 4.2.2.5, 4.2.2.6
- 공식적으로 문서화 되지는 않았지만 운영되고 있다. 



#### 8.8.8.8

- Google이 운영하는 public DNS server

- 공식적으로 문서화 되어있다.
- 8.8.4.4 를 사용해도 된다. 



### DNS Registration and Expiration 

Domain Name은 전세계적으로 고유해야하는데, 각자가 원하는 이름을 할당할 수 있으면 난리가 날 것이다. 



#### Registar

An organization responsible for assigning individual domain names to other organizations or individuals.

- 원래는 몇 Registar 가 없었다. 
- Registar에 가입하고, 원하는 기간동안 비용을 지불하면 된다. 
- Registar의 name serve가 authoritative name server의 역할을 하게 하거나, 직접 개발할 수 있다. 
- 한번 등록된 Domain name은 다른 Registar 에도 알려야한다.
- Registar에서 발급받은 특수한 string과 domain 설정을 TXT record에 기록하곤 한다. 



### Hosts Files 

A host file is a flat file that contains on each line a network address followed by the host name it can be referred to as



#### Loopback address

- A loopback address always points to itself



- Almost every hosts file in existence will in the very least contain a line that reads "127.0.0.1 localhost," most likely followed by "::1 localhost, " where "::1" is the loopback address for IPV6
- Host file을 통한 redirection을 통해 악성행위를 할 수도 있다.



## The Cloud 

### What is the cloud?

Basically, cloud computing is a technological approach where computing resources are provisioned in a shareable way so that lots of users get what they need when they need it.

- With virtualization, a single physical machine called a host could run many individual virtual instances called guests

- A hypervisor is a piece of software that runs and manages virtual machines while also offering these guests a virtual operating platform that's indistinguishable from actual hardware



### Everything as a Service

#### Infrastructure as a Service 

The idea behind infrastructure as a service is that you shouldn't have to worry about building your own network or your own servers.

- AWS
- Infrastructure as a service abstracts away the physical infrastructure you need



#### Platform as a Service

Platform as a service is a subset of cloud computing where a platform is provided for customers to run their services

- Web application은 OS 전체가 필요한게 아니다. Web application을 실행할 수만 있으면 된다. 
-  platform as a service abstracts away the server instances you need.



#### Software as a Service 

 A way of licensing the use of software to others while keeping that software centrally hosted and managed.



### Cloud Storage 

데이터를 로컬에 저장하는 것이 아니라 cloud 에 저장한다. 



## IPv6



### IPv6 Addressing and Subnetting 

#### Internet Protocol version 6

- IPv4는 32bit 주소 공간을 사용했지만, IPv6는 128bit 다. 
- 16bit를 8그룹으로 나눠 표기한다. 



#### IPv6 notation

1. Can remove any leading zero 
2. Any number of consecutive groups composed of just zeros can be replaced with two colons 
   - 이 규칙은 한번만 사용할 수 있다.



- 2001:0db8: 
  - documentation, education 용으로 사용
- ::1
  - loopback address

- FF00::
  - multicast 
- FE80::
  - link-local unicast address
  - Allow for local network segment communications and are configured based upon a host's MAC address



- 첫 64bit는 네트워크 ID이고, 다음 64bit는 호스트 ID로 사용한다.
- subnet은 IPv4와 같은 CIDAR notation을 사용한다. 



### IPv6 Headers

![IPv6 header](https://drive.google.com/uc?export=view&id=1vvxT9jPZTfBODIeURtc6had3wUhDOUkb)

- IPv4와 달리 주소가 길어서 header를 간소화 하려는 노력이 들어갔다.
- Next Header를 사용해 optional한 field는 필요하다면 따로 전달하도록 구현되어 있다. 



### IPv6 and IPv4 Harmony 

모든 인터넷이 IPv4에서 IPv6로 한번에 이동하는 것은 불가능 할 것 이다.

IPv6와 IPv4가 공존할 방법이 필요하다.



#### IPv4 Mapped Address space

- 0:0:0:0:0:ffff:
  - 남아있는 32bit가 IPv4 주소와 같은 역할을 하게된다.
  - IPv4 패킷이 IPv6 network을 돌아다닐 수 있다. 



#### IPv6 tennels 

- IPv6 패킷이 IPv4 네트워크를 돌아다니게 할 수 있는 기술 
- Servers take incoming IPv6 traffic and encapsulate it within traditional IPv4 datagram
- 보내는 쪽과 받는쪽에서 encapsulation 과정을 거친다. 



#### IPv6 tunnel broker 

These are companies that provide IPv6 tunneling endpoints for you, so you don't have to introduce additional equipment to your network.







# REFERENCE

[The Bits and Bytes of Computer Networking - Week6](The Bits and Bytes of Computer Networking)



