---
title: "About"
layout: "about"
url: "/about"
summary: "about"
comments: false
ShowToc: true
---

# Career
[2013.03 - 2019.07] 🎓  서울시립대학교 수학 & 컴퓨터과학 부전공  
[2019.04 - 2019.07] 👔 SecuLetter 인턴 as 머신러닝 엔지니어  
[2019.07 - 2021.03] 👔 SecuLetter Cloud Service Developer

---
## Awards & Certificates
2018 - National Cryptography Competition Encouragement Award  
[2020 - Google The Bits and Bytes of Computer Networking Course](https://coursera.org/share/6af79619653a3c403db9252db9c25bc6)  

---
## Projects
### MacroPredictor - 2019.3 ~ 2019.12
`machine-learning` `python` `tensorflow` `C++` `docker` 
- 문서 파일로부터 악성 Viusal Basic Script를 진단하는 머신러닝 엔진 개발

#### 알게된 점
- 동적 라이브러리와 실행파일간의 IPC 과정 구현
- ElasticSearch 와 Kibana 를 활용한 텍스트 데이터 분석 및 시각화 방법
- 개발자에게는 이미 구현된 프로그램을 과감하게 수정 혹은 폐기 할 수 있는 용기가 필요하다는 점

---
### System Controller - 2020.1 ~ 2020.5

`C++` `CMake` `Linux` `gtest` `Cross-platform` 
- CentOS6 / CentOS8 에서 동작하는 네트워크, NTP, 방화벽등을 관리해주는 시스템 컨트롤러 개발

#### 알게된 점
- CMake를 활용한 C++ 크로스 플랫폼 개발 노하우
- 하나의 실행파일을 구현해 웹과 콘솔 두가지 인터페이스를 제공하는 방법
- 함수 단위의 단위테스트의 중요성

---
### Threat Intelligence Service - 2020.9 ~ 
`spring-boot` `java11` `spring-security` `spring-data-jdbc` `spring-aop` `ELK` `Azure` `docker` `spring-hateoas`
- REST API를 통해 전달된 URL의 악성 여부를 반환하는 서비스 개발

#### 알게된 점
- Spring Boot 를 활용한 REST API 개발 방법
- Azure의 다양한 리소스들을 활용해서 Cloud 환경에 서비스를 배포하는 방법
- 동적라이브러 등을 활용한 소스코드의 모듈화 보다, 마이크로서비스 단위의 개발이 개발자들의 분업에 더 효과적이라는 점
- 주어진 요구사항을 시간내에 구현할 수 있는 능력이 개발자에게 가장 중요한 핵심 역량이라는 것

---

## Personal Proejct
### [LINE-in-action](https://github.com/raeperd/line-in-action) - 2021.3
`typescript` `github-action` 
- Github action for pushing LINE message using LINE-Notify API

#### 알게된 점
- typescript, nodejs의 기본적인 문법들과 탄생 배경
- 새로운 프레임워크에 적응하는 것에 대한 어려움과 그 필요성
- API key를 통해 인증하는 서비스의 경우 API key timeout 등을 응답에 포함하면 사용자가 쓰기 편하다는 점

---

# Language Preference & Proficiency
![language-preference-proficiency](/language-preference-proficiency.png)

---

# Current Interests
- [Kotlin for Java Developers in Coursera](https://www.notion.so/dc85a549771f4ddcae85a42d943b547d) 
    - 코틀린은 자바의 상위호환 언어가 아닌가? 레거시를 유지하는 목적이상으로 자바로 새로운 프로젝트를 시작할 이유가 있을까?
- Spring-Data-JPA 에서 Spring-Data-JDBC 로의 전환
    - Spring Data JPA 에 대한 불만들
        - 어플리케이션 코드에서 보이지 않는 암묵적인 연산과 캐싱
        - Entity 클래스 내부에 final 필드를 정의하기 까다로운 점 (예를 들면 `@Id` 는 final 인게 합리적인데 불가능 하다는 점 등..)
        - NoArgsConstructor 를 강제하는 점
    - 이런 저런 불편보다는 spring-data-jdbc를 쓰는게 더 좋은게 아닐까 하는 고민
- Domain Driven Design
    - 프레임워크와 도메인 로직을 어디까지 분리해야 할까?
        - `@Tranactional` , `@Id` 와 같은 정도의 기능들에 대한 import 정도는 예외적으로 허용해주는게 편한거 아닐까
    - AggregateRoot와 같은 복잡한 개념은 항상 필요한 것일까? 객체 지향에서 피하라던 만능 클래스인게 아닐까?
