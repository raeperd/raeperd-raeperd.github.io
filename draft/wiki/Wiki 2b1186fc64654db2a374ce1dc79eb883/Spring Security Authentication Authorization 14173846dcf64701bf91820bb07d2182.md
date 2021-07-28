# Spring Security Authentication / Authorization

Category: spring-boot
Created: November 18, 2020 6:14 PM
Created By: RaeCheol Park
Last Edited Time: November 18, 2020 6:14 PM
Status: DONE

Spring Security 주요 개념 

- 접근 주체 (Principal) - 보호된 리소스에 접근하는 사용자
    - Authentication 으로 추상화
- 인증 (Authenticate) - 현재 사용자가 누구인지 식별
    - 주요 컴포넌트 - `AuthenticationManager`, `AuthenticationProvider`
- 인가 (Authorize) - 현재 사용자가 보호된 리소스에 권한이 있는지를 검사
    - 주요 컴포넌트 - `AccessDecisionManager`, `AccessDecisionVoter`
- `GrantedAuthority`
    - 인증된 사용자의 인증정보(역할 등)을 표현
- `SecurityContext`
    - 접근 주체(Authentication)와 인증 정보(`GrantedAuthority`)를 담고 있는 Context
    - ThreadLocal에 보관되며, `SecurityContextHolder` 를 통해 접근할 수 있다.

![Spring%20Security%20Authentication%20Authorization%2014173846dcf64701bf91820bb07d2182/Untitled.png](Spring%20Security%20Authentication%20Authorization%2014173846dcf64701bf91820bb07d2182/Untitled.png)

## Spring Security 인증 처리 요약

`SecurityContextPersistenceFilter`

- `SecurityContextRepository`를 통해 `SecurityContext` 를 Load/Save 처리

`LogoutFilter`

- 로그아웃 URL (default: /logout) 로의 요청을 감시하여 해당 사용자를 로그아웃 시킴

`UsernamePasswordAuthenticationFilter`

- ID/Password 기반 Form 인증 요청 URL (default: /login) 을 감시하여 사용자를 인증함

`ExceptionTranslationFilter`

- 요청을 처리하는 중에 발생할 수 있는 예외를 위임하거나 전달

`FilterSecurityInterceptor`

- 접근 권한 확인을 위해 요청을 `AccessDecisionManager`로 위임
- **이 필터가 실행되는 시점에는 사용자가 인증 됐다고 판단**

![Spring%20Security%20Authentication%20Authorization%2014173846dcf64701bf91820bb07d2182/Untitled%201.png](Spring%20Security%20Authentication%20Authorization%2014173846dcf64701bf91820bb07d2182/Untitled%201.png)

## Spring Security 인가 처리 요약

`AccessDecisionManager`

- 인증된 사용자의 보호 리소스 접근 여부를 판단한다. 3개의 기본 구현을 제공한다.
    - `AffirmativeBased` : 접근을 승인하는 voter가 1개 이상
    - `ConsensusBased` : 과반수
    - `UnanimouseBased` : 모든 voter가 승인해야 함

`AccessDecisionVoter`

- `AccessDecisionManager` 는 다수의 `AccessDecisionVoter` 로 구성된다.
- 각각의 Voter는 주어진 정보를 기반으로 승인(ACCESS_GRANTED), 거절(ACCESS_DENIED), 보류(ACCESS_ABSTAIN)를 반환한다.
    - RoleVoter는 보호 리소스에 접근하기 위한 권한을 사용자가 지니고 있는지 확인함
    - WebExpressionVoter는 SpEL 표현식에 따른 웹 접근 제어를 처리함

[Spring Security Reference](https://docs.spring.io/spring-security/site/docs/5.4.1/reference/html5/#servlet-authentication-providermanager)

![Spring%20Security%20Authentication%20Authorization%2014173846dcf64701bf91820bb07d2182/Untitled%202.png](Spring%20Security%20Authentication%20Authorization%2014173846dcf64701bf91820bb07d2182/Untitled%202.png)

Spring Boot provides a default global `AuthenticationManager` (with only one user) unless you pre-empt it by providing your own bean of type `AuthenticationManager`. The default is secure enough on its own for you not to have to worry about it much, unless you actively need a custom global `AuthenticationManager`. If you do any configuration that builds an `AuthenticationManager`, you can often do it locally to the resources that you are protecting and not worry about the global default.

> The fact that all filters internal to Spring Security are unknown to the container is important, especially in a Spring Boot application, where, by default, all @Beans of type Filter are registered automatically with the container. So if you want to add a custom filter to the security chain, you need to either not make it be a @Bean or wrap it in a FilterRegistrationBean that explicitly disables the container registration.