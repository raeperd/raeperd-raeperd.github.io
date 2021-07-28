---
title: "Spring Runner day3"
date: 2020-07-17T19:30:04+09:00
categories: [""]
subcategories : [""]
tags: [""]
draft: true
---



domain 영역은 퓨어한 자바로 작성하는게 이상적인 상황이다.

ReadableErrorAttributes

Locale을 동적으로 변환하려면 LocaleResolver 라는걸 구현해서 쓰면 된다.



@ConfigurationPropertiesScan

- @ConfigurationProperties가 붙어있으면 빈에 등록할 수 있다,  



인증

- 건물에 들어갈 자격

인가

- 방에 들어갈 자격 



HttpForm을 받아오는 방법

1. HttpServletRequest, request.getParameter("username")
2. @RequestParam 
   - 변수의 이름으로 param의 이름을 알수 있어 생략가능
   - java primitive type은 RequestParam 으로 올것이라 @RequestParam 자체도 생략 가능하다.
3. Java bean을 사용하는 방법
   - 자료를 담는 클래스를 만든다. 
   - RequestBody를 가져오는게 아니라 form을 가져오는거라 그냥 쓰기만 하면 된다. 



스프링 servlet spec 과 reactive spec 이 존재한다.

servlet spec에 의존성을 가지면 spec을 변경하기 힘들다.



RedirectView



자바의 문법으로 예외를 처리하기 vs @ExceptionHandler 를 통해 예외 처리 



ExceptionHandler 의 생존 주기

- Controller 의 내부



@ControllerAdvice

- 모든 컨트롤러에 영향을 미치고 싶을때,



BindingResult 의 유무

- 있다. 개발자가 이걸 처리하는구나.
- 없다. 이건 만족안하니까 예외를 던져야겠다. 
- @Valid로 선언된 파라미터의 바로 뒤에 전달 되어야 한다.



핸들러마다 Command 객체를 만드는 것이 BestPractice 

중복되는 영역이 많을 수 있다.

인터페이스로 의도를 밝힐 수도 있다.



로그인이 되어있는 사용자는 세션으로 관리할 수 있다.	



세션은 서버가 하나일때는 아무런 문제가 없다.

StickySession

redis 와 같은 인메모리 데이터베이스에 Session을 저장한다.

session 객체를 사용해버리면 나중에 또 변경사항에 유연하지 못하다.

Session을 추상화시킨다.



```java
	@GetMapping("/api/user/profile")
	public ResponseEntity<UserProfile> userProfile() {
		UserSession session = sessionRepository.get();
		if (Objects.isNull(session)) {
			return ResponseEntity.status(HttpStatus.UNAUTHORIZED).build();
		}
		return ResponseEntity.ok(new UserProfile(session.getUser()));
	}
```



HandlerMethodArgumentResolver

addArgumentResolver 



Autowired vs 생성자 의존 주입 



요청 처리 전 후에 처리할 기능이 필요할 때, 

- HandlerInterceptor
- WebRequestInterceptor: 요새는 잘 안쓴다. 
- 서블릿 실행 전 후 처리를 위한 컴포넌트
  - Servlet Filter 



사용자의 권한은 db로 부터 가져와서 세션을 만드는게 이상적이다.



@Autowired 는 빈으로 등록된 개체에만 적용할 수 있다. 

Container가 Autowired를 해석할 수 있다.



ResponseStatusExceptionResolver



Servlet이 이미 인증과 인가가 가능하다

- request.getUserPrinciple
- request.isUserInRole



servlet spec에 의존하지 않고 Session을 처리하는건 또 다르게 어려운일 



you are not gonna need it

springboot가 너무 많은걸 



사용자의 프로필 사진을 서버에서 변경하더라도 세션에서 변경하지 않으면 사용자는 알 수 없다.

Profile을 통해 개발 환경과 배포 환경을 구분할 수 있다.

- @Conditional(ProfileCondition.class)



html5 server sent events

Tomcat의 default thread는 200개



```java
@RequestMapping("/stream/online-user-counter")
	public void count(HttpServletResponse response) throws IOException, InterruptedException {
		response.setContentType("text/event-stream");
		
		for (int count = 0; count <= 10; count++) {
			OutputStream outputStream = response.getOutputStream();
			outputStream.write(("data: " + count + "\n\n").getBytes());
			outputStream.flush();
			
			Thread.sleep(1000);
		}
	}
```

이렇게 쓰면 커넥션 하나당 쓰레드 하나를 쓰게 되버린다.

Reactive를 사용하면 이렇게 하지 않는다.

Nonblocking IO 





# ?

Form 전송 





















