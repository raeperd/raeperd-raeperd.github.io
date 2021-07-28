---
title: "Spring Runner Week1"
date: 2020-07-17T19:30:04+09:00
categories: [""]
subcategories : [""]
tags: [""]
draft: true
---



# INTRO





# CONETNS

엔터프라이즈 자바는 스펙에 가깝다

여러 구현체가 제공된다. 

Tomcat 



스프링 핵심 기술

1. 제어 역전
2. 관점 지향 프로그래밍 
3. 이식 가능한 서비스 추상화



메시지를 전달받을떄 스펙을 정하면 메시지 specific한 오브젝트가 되버린다.

POJO 

순수하게 자바로 작성을 하자 ?

다른 annotation을 변경함으로써 규약과 환경에 적응할 수 있다.

spring framework 이 구현하지 않은 메시지 스펙은 못쓰는거 아닌가??

- POJO 를 지키면서 추가 구현을 통해 사용할 수 있다. 



Ioc 컨테이너가 관리하는 빈 



이식 가능한 서비스 추상화 



횡단관심사

어플리케이션 전체에서 필요한 기능들



웹 브라우저는 기본적으로 POST 와 GET을 지원한다

HTML form 전송



## 자바 웹 

자바 서블렛, Java server page 

자바 서블릿은 자바코드로 웹을 만들고

jsp는 자바와 html을 함꼐 사용해서 웹을 만든다.

@ServletComponent



jsp -> 자바 클래스로 변경 -> 서블릿 

자바와 html을 섞어서 쓰면 유지보수하기 짜증난다.



request에 requestName 속성을 담아 준다.

${requestName} 로 접근할 수 있다.

html은 순수하게 뷰의 역할만 하게 할 수 있다. 



## 스프링 웹 



로직과 프레젠테이션 분리



모델 화면의 구성 요소 

View 화면 출력 요소

Contoroller 화면을 구성하는 로직 

재사용 수준에 따라 모델을 분리하기도 하고 안하기도 하지만 컨트롤러 내부에서 정의하기도 한다. 



Reflection api 

왜 이런짓을 하는거지??



start.spring.io



controller는 model과 view를 리턴한다

model 과 controller는 분리하지 않나?



spring:eval anotation을 해석해서 출력하도록 한다.

setViewName vs setView 



HTTP 요청을 처리할 목적으로 작성한 메소드를 핸들러라고 부른다

핸들러는 모델과 뷰를 반환해야한다



## Application Architecture



아키텍처 정의

시스템의 구성 요소를 어떤 원칙하에 분리하고, 구성 요소 간의 상호작용 방법에 대한 것



interface 를 구현한 것이 adaptor



포트 및 어댑터 아키텍처

도메인 주도 개발에서 언급되곤 한다.



클린 아키텍처 



MVC

플랫폼마다 해석이 다양하다



REST 아키텍처 



확장성 있는 웹 어플리케이션 아키텍처 

웹에서는 스케일 아웃이 더 주요하게 사용된다. 







# REFERENCE



