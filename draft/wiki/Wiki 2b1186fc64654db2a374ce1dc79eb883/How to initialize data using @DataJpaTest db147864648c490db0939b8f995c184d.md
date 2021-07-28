# How to initialize data using @DataJpaTest

Category: spring-boot
Created: November 2, 2020 8:52 PM
Created By: RaeCheol Park
Last Edited Time: November 2, 2020 8:52 PM
Status: NEED_DETAIL

Repository를 테스트 하기 위해서는 당연하게도 DB에 데이터를 넣음으로써 원하는 상황(state)을 만들어야한다.

모든 테스트는 각각 나름의 state를 필요로 한다. 다른 테스트와 마찬가지로 초기화하는 과정이 길어지면 테스트 코드의 가독성이 나빠진다.

data.sql 과 같이 파일을 분리해서 초기 상태를 지정하는 방법이 있다.

아주 많은 데이터를 초기화해야 하는 경우 적합하다고 할 수 있지만, 이렇게 구현하는 경우 모든 테스트가 data.sql 파일에 의존성을 가지게 된다. 각각의 테스트는 모두 독립적인 것이 이상적이지만, 하나의 파일로 모든 상태를 관리하는 것은 어려우며, 여러 파일을 이용해서 그때 그때 필요한 파일들을 직접 활용해 실행할 수 있다.

```java
@Test
@Sql(scripts = {"/import_senior_employees.sql"}, 
  config = @SqlConfig(encoding = "utf-8", transactionMode = TransactionMode.ISOLATED))
public void testLoadDataForTestCase() {
    assertEquals(5, employeeRepository.findAll().size());
}
```

아직까지 복잡한 state 안에서의 단위테스트가 필요해본 적은 없어서 그런지 ,내 경우는 그냥 아주 기분이 되는 필수적인 데이터들은 data.sql에 초기화를 하고 (기껏해야 2~3개) 추가적으로 필요한 데이터의 경우 직접 넣어서 테스트 하는 형식으로 구현하고 있다.

그런 상황이 오면 파일을 분리하고, 그룹으로 관리하는 방법을 생각해 볼 법 할 것 같다.

[SpringBoot 1.4.0 Test 적용하기 (1)](https://jojoldu.tistory.com/33)

[Guide on Loading Initial Data with Spring Boot | Baeldung](https://www.baeldung.com/spring-boot-data-sql-and-schema-sql)