---
title: "Query Did Not Return a Result 2"
date: 2020-07-21
tags: ["java", "spring", "bug"]
---

# 현상
`JpaRepsotiroy` 에서 기본적으로 제공하는 인터페이스를 사용할떄, query 결과가 여럿 일 수 있는데 반환형을 하나의 엔티티만 담을 수 있게 하면 아래와 같은 오류를 만나게 된다.

```
query did not return a unique result: 2
```

# 해결과정
제일 외부에서 확인할 수 있는 예외는 심지어 `UnknownHostException` 이었는데 원인이 여기있다는 것을 알게 되기 까지 꽤나 오랜 시간이 필요했다.

Entitiy를 만들 떄 unique constraint를 줬기 때문에 이런 오류가 발생할 리가 없다고 생각했었다. constraint를 pair로 만들었기 때문에 단일 컬럼은 충분히 여러 값이 나올 수 있는 거였음

그런데 난 Repsotory 레벨에서부터 단위테스트를 하면서 Controller 까지 만든거라서 repository 레벨에서 문제가 있을 것이라고는 생각하기 어려웠었다. 

Spring Data jpa 에서는 이런 것을 단지 method 이름만으로 제한할 수 있다.

 `findTop{num}By` `findFirst{num}By` 와 같은 방식으로 제한할 수 있다. 하나만 있으면 되는 상황에서 Top1 과 같은 네이밍은 뭔가 모호한 것 같고, findFirst 와 같은 네이밍이 적절해보인다. 

constraint 가 이렇든 저렇든, 하나의 값만을 원하는 상황이라면 쿼리를 제한하는 키워드는 반드시 필요하다.

사용하면 할수록 Spring Data나 Jpa는 정말 잘 만든 라이브러리인것 같다. 나는 이런 레벨의 라이브러리를 만들 수 있을까 싶다