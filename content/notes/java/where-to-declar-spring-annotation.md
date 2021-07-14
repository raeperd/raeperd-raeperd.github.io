---
title: "Where to Declar Spring Annotation"
date: 2020-11-02
tags: ["java", "spring", "bug"]
---

`@WebMvcTest` 어노테이션만 선언했는데 Jpa Auditing 이 정상동작 하지 않는 문제를 발견했다.

원래 annotation을 붙일때 크게 생각을 하지 않고 붙였는데, `@WebMvcTest` 를 선언하면서 src 폴더를 기준으로 spring이 component를 스캔하는 과정에서 main 클래스를 읽으면서 오류가 발생한듯 하다.

 `@SpringBootApplication` 과 같이 다른 설정을 읽으려다가 오류가 나는 것으로 보인다. (annotation의 위치를 옮기면 정상 동작했다. 이 경우는 `@EnableJpaAuditoring` 이었다.) 
 
 이 과정에서 괜한 annotation까지 읽게 되면서 테스트 환경에서 문제가 발생한다. 
단 한줄의 annotation이라도 따로 @Configuration 클래스를 만들어서 따로 관리하는 것이 옳다. 
공식 reference 에서도 이를 권장하는 것으로 보인다.  

비슷하게 아래와 같은 configuration 도 오류를 쉽게 발생시킨다.
```java
package com.seculetter.intelligencebatch.configuration;

import org.springframework.context.annotation.Configuration;
import org.springframework.data.jpa.repository.config.EnableJpaAuditing;
import org.springframework.data.jpa.repository.config.EnableJpaRepositories;

@EnableJpaAuditing
@EnableJpaRepositories
@Configuration
public class DataJpaConfiguration {
}
```
- By default, Spring Boot enables JPA repository support and looks in the package (and its subpackages) where @SpringBootApplication is located. If your configuration has JPA repository interface definitions located in a package that is not visible, you can point out alternate packages by using @EnableJpaRepositories and its type-safe basePackageClasses=MyRepository.class parameter.
- `@EnableJpaRepositories` should not be used in sub package. Spring confuse where to search JPA repositories.

## Thanks to  
[Accessing Data with JPA](https://spring.io/guides/gs/accessing-data-jpa/)  
[Spring @WebMvcTest with @EnableJpa* annotation](https://stackoverflow.com/a/51558289/13662830)  
[Spring Boot Features](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-testing)