# How to resolve spring security exception to ExceptionHandler

Category: spring-boot
Created: November 2, 2020 9:26 PM
Created By: RaeCheol Park
Last Edited Time: November 2, 2020 9:26 PM
Status: NEED_DETAIL

Thanks to

[How to manage exceptions thrown in filters in Spring?](https://stackoverflow.com/a/55864206/13662830)

Spring Securiy에서의 예외는 RestController 내부의 @ExceptionHandler 메소드로 Resolve 는 되지 않는데, RestControllerAdvice를 이용하면 사용 할 수 있었다.

이유를 생각해보자면, filter 이후에 DispatcherSevelet이 요청 URL을 Controller로 매칭하기 때문에 Controller 내부에 어떤 ExceptionHandler 들이 있든지, resolve할 수 없을 것이다. 구조상 당연한 것이었다.