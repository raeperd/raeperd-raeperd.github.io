---
title: "Spring Runner Week1"
date: 2020-07-17T19:30:04+09:00
categories: [""]
subcategories : [""]
tags: [""]
draft: true

---





spring data jpa

h2 daabase

thymeleaf

spring web



Description	Resource	Path	Location	Type
Project at 'C:\Users\raepe\Documents\workspace-spring-tool-suite-4-4.7.0.RELEASE\demo' can't be named 'todo' because it's located directly under the workspace root. If such a project is renamed, Eclipse would move the container directory. To resolve this problem, move the project out of the workspace root or configure it to have the name 'demo'.	demo		line 0	Gradle Error Marker





```yaml
server.port: 8089

logging.level;
	org.springframework.boot: INFO
	org.springframework.web: DEBUG
```



WebMvcConfigurere를 구현한 클래스를 빈에 추가하면 내 설정을 spring boot에 알려줄 수 있다. 

- ex @Override addResourceHandler



springboot 는 webapp 디렉토리를 사용할수없고 classpath를 사용하는것이 기본이다.

자원을 획득할때 Resource로 부터 자원을 획득한다

springmvcConfigurer



handler



자동 탐색과 빈 컨트롤



빈을 등록 안해도 니가 알아서 컴포넌트를 찾아서 등록해줘 

@ComponentScan 

@SpringBootApplication 이 있는 패스로 부터 탐색을 시작

- @Component 를 기준으로 한다
- @Controller 내부에 있다 
- Reflection API 를 이용해서,



ViewResolver interface를 통해 View 를 찾는다.

- 논리적인 View의 이름으로부터 view 인스턴스를 반한한다.



springboot 가 사용자가 thymeleaf 를 쓴다는 걸 알게되면, Thymeleaf view resolver를 사용한다.

그냥 String을 리턴하면 이게 논리적인 뷰의 이름으로 이해한다.

Model은 파라미터로 정의해서 사용할 수도 있다.



```java
	@RequestMapping("/todos")
	public ModelAndView todos() {
		
		SiteProperties site = new SiteProperties();
		site.setAuthor("raeperd");
		site.setDescription("in workshop");
		
		return new ModelAndView("todos", "site", site);
	}
```



```java
	@RequestMapping("/todos")
	public String todos(Model model) {
		
		SiteProperties site = new SiteProperties();
		site.setAuthor("raeperd");
		site.setDescription("in workshop");
		
        model.addAtribute("site", site);
        return "todos"
	}
```



```java
@RequestMapping("/todos")
	public void todos(Model model) {
		
		SiteProperties site = new SiteProperties();
		site.setAuthor("raeperd");
		site.setDescription("in workshop");
		
        model.addAtribute("site", site);
        // return "todos"
	}
```



명시적으로 string을 리턴하는 방식과 보이드를 리턴하는 방식 

매번 실행할때마다 탐색을 하게 된다. 



@AutoWired 

- 이런 의존성이 있습니다. 생성할때 container 에서 이 의존성을 찾아서 주입해주세요 



Environment 객체에 application.yml의 정보를 등록한다.



1. Evironment를 이용해서 가져온다
2. @Value 
3. 



1. @Component
2. @Bean 등록
3. @ConfigurationPropertiesScan



java의 프로퍼티 

멤버 변수와 프로퍼티의 차이



application.yml 에서 site.author를 가져올때 ,

- author는 멤버 변수의 이름이 아니라 property 의 이름이다.
- 이렇게 하자는 약속 



html을 전달해주는 것과 json을 전달해주는 것



spring이 응답하는 방식

1. view를 반환하는 방식
2. json, xml 형태를 반환



@ResponseBody

- 클라이언트가 요구하는 구조대로 내 리턴을 출력해줘



```
2020-07-18 14:12:15.556 DEBUG 1000 --- [nio-8080-exec-9] m.m.a.RequestResponseBodyMethodProcessor : Using 'application/json', given [application/json, text/plain, */*] and supported [application/json, application/*+json, application/json, application/*+json]
```



```java
@Controller
public class TodoRestController {

	@RequestMapping("/api/todos")
	public @ResponseBody List<Todo> todos() {
		return Collections.emptyList();
	}
}
```



```java
@RestController
public class TodoRestController {

	@RequestMapping(value = "/api/todos", method = RequestMethod.GET)
	public @ResponseBody List<Todo> todos() {
		return Collections.emptyList();
	}
}
```



```java
@RestController
public class TodoRestController {

	@GetMapping("/api/todos")
	public @ResponseBody List<Todo> todos() {
		return Collections.emptyList();
	}
}
```



@RestController = @Controller + @ResponseBody



```java
@Controller
public class TodoRestController {
	
	private TodoFinder finder;
	
	public TodoRestController(TodoFinder finder) {
		this.finder = finder;
	}
	
	@RequestMapping("/api/todos")
	public @ResponseBody List<Todo> todos() {
		return this.finder.getAll();
	}
}
```



TodoFinder 는 어떻게 찾을까?

TodoManager가 TodoFinder를 구현

TodoManager의 @Service

@Service의 @Component 



@Primary

@Qualifier("todoManager")



```java
@RestController
public class TodoRestController {
	
	private TodoFinder finder;
	
	public TodoRestController(TodoFinder finder) {
		this.finder = finder;
	}
	
	@GetMapping("/api/todos")
	public List<Todo> todos() {
		return this.finder.getAll();
	}
}
```



InitializingBean

CommandlineRunner

ApplicationRunner



빈의 순서가 중요한 경우는 별로 좋은 설계는 아닐 것이다.

DisposableBean 

- distroy는 빈에만 의존하는게 좋을 것



SmartLifecycle 

- start stop end 
- kafka 와 같은 메시징 어플리케이션의 경우 사용의 여지가 있음



```
	@PostMapping("/api/todos")
	@ResponseStatus(HttpStatus.CREATED)
	public void add(@RequestBody JsonNode node) {
		editor.create(node.get("title").asText());
	}
```

구체적 구현에 의존했다.

나중에 바이너리로 바꾼다거나 다른 json 라이브러리를 사용한다면 ??



@RequestBody를 보고 읽어야하는구나,

그걸 TodoWriterCommand 에게 전달해줘야하는구나 

ContentsType을 보니 json이다. 이거 변환해줄수 있는 Converter가 뭐가 있지?

Jakson 의 JsonToHttpBody

어떻게 읽어야하냐면, TodoWriterCommand의 프로퍼티를 볼꺼야



ContentsNegotiation 



```java
	@DeleteMapping("api/todos/{id}")
	@ResponseStatus(HttpStatus.ACCEPTED)
	public void delete(@PathVariable("id") Long id) {
		editor.delete(id);
	}
```

변수의 이름으로 pathvariable을 유추할수 있지 않을까??



java bean validation api

- jakarta bean validation



IME ???



같은 URL인데 요구하는 타입이 다른 경우

- Contents Negotiation
- 협상이 실패하면 안보내지는 않는다. 최소한의 성의는 보인다.



csv로 주고싶다.

- CsvView를 구현
- CsvViewResolver 를 구현
- SpringBoot에게 새로운 ViewResolver를 알려준다.



ContentsNegotiationManager

CotentsNegotiationViewResolver



가능한 모든 ViewResolver에게 논리적 View의 존재 여부를 물어본다.

가능한 모든 View를 가져온다

`+` default view

어떤 ContentsType이니? 



CotentsNegotiationViewResolver



새로운 View와 새로운 ViewResolver를 구현하는 것ㅇ ㅣ아니라 default View의 목록을 추가한다.

default Veiw에 Mapping 

View의 논리적 이름은 ViewResolver에게만 유효하다.

ContentsType 



controller가 뷰의 논리적 이름을 반환한다.

CotentsNegotiationViewResolver 가 가능한 뷰를 찾는다. ViewReslover 들에게 물어본다

거기에 defaultView를 더한다. 

ContentsNegotiation을 통해 타입을 확인한다. 



defaultView를 만든다. model을 이용해서

model이 적절하지 않다. 

RestController는 defaultView의 영향을 받지 않는다.

defaultView



resolveErrorView

- 404는 이렇게
- 405는 저렇게



defaultErrorAttributes

ErrorAttribute를 상속받아서 구현하고, bean에 등록한다.



ReadableErrorAttributes

- decorator pattern



어떻게 사용자가 정의한 ErrorAttribute를 인지할 수 있는거지 ?

- ErrorMvcAutoConfiguration을 Springboot 가 사용한다. 
  - @ConditionalOnMissingBean
  - @ConditionalOn 은 다 boot 
  - 사용자가 정의한 ErrorAttributes이 없다면, DefaultErrorAttribute를 사용한다. 





















