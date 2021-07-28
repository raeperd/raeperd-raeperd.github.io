[현상]

유효한 CURL 요청이지만, 브라우저에서 swagger-ui 를 만드려면 CORS 이슈 때문에 리소스를 가져오지 못하는 현상



[원인]

- 기존에 Threat intelligence 로 오는 요청들은 모두 브라우저를 거치지 않은 요청이었음

- Threat intelligence 서비스의 서버사이드에서 CORS 기능 미지원



[해결방안]

- GA 인증이 필요한 API에 한해서만 CORS 기능을 지원하도록 구현



[참고사항]

https://evan-moon.github.io/2020/05/21/about-cors/ 

https://spring.io/blog/2015/06/08/cors-support-in-spring-framework 

https://spring.io/guides/gs/rest-service-cors/ 



https://oddpoet.net/blog/2017/04/27/cors-with-spring-security/ 

- *CORS preflight* 요청은 인증처리를 하지 않아야 한다. CORS semantic 상으로 CORS prefight에는 Authorization 헤더를 줄 이유가 없으므로 *CORS preflight* 요청에 대해서는 401 응답을 하면 안된다.



Swagger-ui 에서도 이런 문제를 지적하고 있음. 참고 →

[swagger-ui/cors.md at master · swagger-api/swagger-ui](https://github.com/swagger-api/swagger-ui/blob/master/docs/usage/cors.md)

