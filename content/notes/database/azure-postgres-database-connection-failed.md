---
title: "Azure Postgres Database Connection Failed"
date: 2021-09-21
tags: ["database", "bug"]
---

# 현상
```java
2020-11-05 16:23:57.380  INFO 4608 --- [           main] c.s.i.IntelligenceBatchApplication       : Starting IntelligenceBatchApplication on DESKTOP-1L9DR23 with PID 4608 (D:\git\intelligence-batch\build\classes\java\main started by raepe in D:\git\intelligence-batch)
2020-11-05 16:23:57.383  INFO 4608 --- [           main] c.s.i.IntelligenceBatchApplication       : The following profiles are active: release
2020-11-05 16:23:57.778  INFO 4608 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Bootstrapping Spring Data JPA repositories in DEFERRED mode.
2020-11-05 16:23:57.824  INFO 4608 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Finished Spring Data repository scanning in 39ms. Found 2 JPA repository interfaces.
2020-11-05 16:23:58.103  INFO 4608 --- [           main] o.s.s.concurrent.ThreadPoolTaskExecutor  : Initializing ExecutorService 'applicationTaskExecutor'
2020-11-05 16:23:58.109  INFO 4608 --- [           main] o.s.s.c.ThreadPoolTaskScheduler          : Initializing ExecutorService 'taskScheduler'
2020-11-05 16:23:58.172  INFO 4608 --- [         task-1] o.hibernate.jpa.internal.util.LogHelper  : HHH000204: Processing PersistenceUnitInfo [name: default]
2020-11-05 16:23:58.208  INFO 4608 --- [         task-1] org.hibernate.Version                    : HHH000412: Hibernate ORM core version 5.4.21.Final
2020-11-05 16:23:58.224  WARN 4608 --- [           main] o.s.b.a.batch.JpaBatchConfigurer         : JPA does not support custom isolation levels, so locks may not be taken when launching Jobs
2020-11-05 16:23:58.229  INFO 4608 --- [           main] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Starting...
2020-11-05 16:23:58.312  INFO 4608 --- [         task-1] o.hibernate.annotations.common.Version   : HCANN000001: Hibernate Commons Annotations {5.1.0.Final}
2020-11-05 16:23:59.395 ERROR 4608 --- [           main] com.zaxxer.hikari.pool.HikariPool        : HikariPool-1 - Exception during pool initialization.

org.postgresql.util.PSQLException: The connection attempt failed.
```

# 해결방안
datasource 를 로컬의 postgresql 에 연결했을때는 정상적으로 동작했는데, azure 의 postgres 에 연결하면 오류가 발생하면서 동작을 하지 않았다.

datasource 정보를 잘못 입력한게 가장 유력한 후보였는데 이것도 아니었다. 

같은 datasource를 사용하는 다른 프로젝트는 정상적으로 동작을 해서 비밀번호의 대소문자라던지 인코딩이라던지 이런 문제인가 싶어 이쪽으로 검색을 해봤었다.

알고보니 postgresql 의 드라이버의 문제였다. 가장 핵심이 되었던 건 아래의 stack overflow 심지어 한달전의 질문이었음

[Can not connect to Postgres database on Azure with Java](https://stackoverflow.com/questions/64016297/can-not-connect-to-postgres-database-on-azure-with-java)  

`gssEncMode` 라는 옵션을 바꿨더니 되더라는 얘기였는데, 이 질문을 보고나니 릴리즈 노트를 확인해야겠다 싶었다. 아래의 로그를 확인 할 수 있었다.

---
## **Version 42.2.17 (2020-10-09)**
**Notable changes**
### **Changed**
- **Change default of gssEncMode to ALLOW. PostgreSQL can deal with PREFER but there are cloud providers that did not implement the protocol properly.** Libpq gets around this by checking for a GSS credential cache before attempting the connection. This is possible in JDK 8 and up, but not JDK6, or JDK7 fixes Issue #1868 [PR #1913](https://github.com/pgjdbc/pgjdbc/pull/1913)
---
[PostgreSQL JDBC Changelog](https://jdbc.postgresql.org/documentation/changelog.html)

# 교훈

어쩔수 없는 오류인가 싶었는데 사실 아니었다.

gradle 의 dependency를 보통 새로 만드는 프로젝트에서는 버전을 명시하지 않으면 최신 버전을 가져오게 되는데 프로젝트 진행 중에 의존성 라이브러리가 변화한다면 다른 환경에서 다시 gradle 을 통한 빌드를 했을 때 같은 동작을 보장하지 않을 수도 있다.

그래서 프로젝트가 어느정도 진행이 되고 나서는 의존성의 버전을 고정해 주는 것이 필요한데 그 시점을 언제로 할지가 조금 애매한 것 같다. 

이 경우엔 아직 첫 릴리즈도 되지 않은 프로젝트 였는데 사실 첫 릴리즈 브랜치를 딸 때 의존성 라이브러리들의 버전을 현재 최신의 버전으로 명시를 하고, QA 를 시작하는 것이 장기적으로 좋을 것 같다.