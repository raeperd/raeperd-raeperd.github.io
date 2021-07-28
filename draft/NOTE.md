# DNSrpz?

https://dnsrpz.info/

---



# Jackson

``` java
package com.seculetter.threat_intelligence.infrastructure.malwarebazaar;

import com.fasterxml.jackson.annotation.JsonProperty;

class MalwareBazaarResponseDTO {

    private final MalwareBazaarQueryStatus queryStatus;

    MalwareBazaarResponseDTO(@JsonProperty("query_status") MalwareBazaarQueryStatus queryStatus) {
        this.queryStatus = queryStatus;
    }

    MalwareBazaarQueryStatus getQueryStatus() {
        return queryStatus;
    }

}

```

왜 이렇게 해야만 동작하는가



https://stackoverflow.com/questions/24157817/jackson-databind-enum-case-insensitive/44217670

https://www.baeldung.com/parameterized-tests-junit-5

https://stackoverflow.com/questions/43769301/how-to-customize-springwebflux-webclient-json-deserialization



bitbucket-pipeline devops

https://community.atlassian.com/t5/Bitbucket-questions/Bitbucket-Pipeline-Using-a-different-environmental-variable/qaq-p/1023246

https://support.atlassian.com/bitbucket-cloud/docs/variables-and-secrets/

https://community.atlassian.com/t5/Bitbucket-Pipelines-questions/YAML-anchors-Overriding-Reusing-deployment-variables/qaq-p/1367117



logstash logback

https://www.baeldung.com/logback

https://www.elastic.co/kr/blog/logstash-metadata



``` xml
    <springProfile name="release">
        <appender name="logstash-release" class="net.logstash.logback.appender.LogstashTcpSocketAppender">
            <destination>52.231.26.74:5000</destination>
            <encoder class="net.logstash.logback.encoder.LogstashEncoder" >
                <customFields>{"@metadata":{"profile":"release"}}</customFields>
            </encoder>
            <keepAliveDuration>1 minutes</keepAliveDuration>
        </appender>
    </springProfile>
```

customField 는 유효한데 parsing 을 실패한다.

```
Caused by: java.lang.IllegalStateException: Logback configuration error detected: 
ERROR in net.logstash.logback.composite.GlobalCustomFieldsJsonProvider@49bf29c6 - Failed to parse custom fields [{"@metadata":{"profile":"release"}] com.fasterxml.jackson.core.io.JsonEOFException: Unexpected end-of-input: expected close marker for Object (start marker at [Source: (String)"{"@metadata":{"profile":"release"}"; line: 1, column: 1])
 at [Source: (String)"{"@metadata":{"profile":"release"}"; line: 1, column: 35]
```



https://github.com/logstash/logstash-logback-encoder/issues/168

logback-classic 의 버그인데 새로운 버전에서 해결되었다고 하지만, 구현사항이 바뀐것 같다. 단순히 버전을 올리면 다른 의존성 문제로 버그를 만들기 때문에서 사용할 수 없음



logback.conf 를 수정해야함

``` 
input {
	beats {
		port => 5044
	}

	tcp {
		port => 5000
		codec => json_lines
	}
}

filter {
	mutate { add_field => {"[@metadata][profile]" => "%{profile}"} }
	mutate { remove_field => "profile" }
}

## Add your filters / logstash plugins configuration here

output {
	stdout { codec => rubydebug { metadata => true } }
	if [@metadata][profile] {
		elasticsearch {
		hosts => "elasticsearch:9200"
		index => "threat-intelligence-%{[@metadata][profile]}-%{+YYYY.MM.dd}"
		user => "elastic"
		password => "changeme"
		ecs_compatibility => disabled
		}	
	}
}
```

https://www.elastic.co/guide/en/logstash/current/event-dependent-configuration.html



elasticsearch clone vs reindex



---

SameSite Lax

```
POST /_aliases
{
  "actions": [
    {
      "add": {"index": "threat-intelligence-master*", "alias": "sample_aliais"}
    }
    ]
}
```

subzone vs subdomain



https://cloud.google.com/blog/topics/developers-practitioners/comparing-containerization-methods-buildpacks-jib-and-dockerfile



https://www.youtube.com/watch?v=j5a4K0u1HOw



https://stackoverflow.com/questions/4775379/using-not-operator-in-if-conditions



# Flyway

https://www.baeldung.com/database-migrations-with-flyway

https://flywaydb.org/documentation/usage/plugins/springboot.html

https://docs.spring.io/spring-boot/docs/current/reference/html/howto.html#howto.data-initialization.migration-tool.flyway

https://flywaydb.org/documentation/concepts/migrations



```html
<mark>
  highlight!!
</mark>
```



https://stackoverflow.com/questions/53172123/flyway-found-non-empty-schemas-public-without-schema-history-table-use-bas





https://docs.spring.io/spring-data/jdbc/docs/2.2.2/reference/html/#jdbc.entity-persistence



# database index 

https://stackoverflow.com/questions/1108/how-does-database-indexing-work



(참고)

빠른 쿼리를 위해 where 절에 사용되는 컬럼에 대해 인덱스를 생성하면 좋습니다. 기본적으로 btree로 생성됩니다.

url 테이블을 예로 들면,

```
1// address 만으로 조회하는 쿼리를 위한 인덱스 2CREATE INDEX [IF NOT EXISTS] 인덱스명 ON url (address); 3 4// address, vendor_name 조합으로 조회하는 쿼리를 위한 인덱스 5CREATE INDEX [IF NOT EXISTS] 인덱스명 ON url (address, vendor_name); 6 7// address, malicious 조합으로 조회하는 쿼리를 위한 인덱스 8// e.g. SELECT u.* FROM url u WHERE u.address='...' and u.malicious=true; 9CREATE INDEX [IF NOT EXISTS] 인덱스명 ON url (address, malicious); 10 11// malicious 컬럼만으로 조회하는 쿼리를 위한 인덱스 12CREATE INDEX [IF NOT EXISTS] 인덱스명 ON url (malicious); 
```

인덱스를 정의해두면 조회 속도는 빨라질 수 있지만, 레코드의 변경 시 마다 인덱스를 업데이트하는 비용이 발생하므로, 너무 많은 인덱스를 정의하는 것은 피해야합니다.

인덱스 생성 전과 생성 후의 Query Plan을 EXPLAIN 구문으로 비교해보면, 효과를 추정해볼 수 있습니다.





# Kotlin 

https://www.youtube.com/watch?v=_hfBv0a09Jc



http://20.194.18.41:5601/app/lens#/?_g=(filters:!(),refreshInterval:(pause:!t,value:0),time:(from:now-1d,to:now))



32651



https://peterevans.dev/posts/how-to-host-swagger-docs-with-github-pages/

