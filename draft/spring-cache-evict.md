 Virustotal API KEY의 경우 spring cache 를 이용한 caching 을 활용하고 있었는데, 새로운 키를 추가하거나 삭제하는 경우에도 cache를 버리지 않는 것처럼 동작하는 것을 확인함 



수정이 필요할 것으로 보임



이 기능이 동작하지 않았던 원인은 @CacheEvict annotation의 allEntries의 값을 true 로 세팅하지 않았기 때문임



CachaManager 를 주입받아 테스트 할 수 있었는데 이를 하지 않아 발생한 오류 



어떤 메소드를 캐싱해야하는지는 [Spring Data REST Reference Guide / 4.2. The Collection Resource](https://docs.spring.io/spring-data/rest/docs/current/reference/html/#repository-resources.collection-resource) 를 참고



[A Guide To Caching in Spring | Baeldung](https://www.baeldung.com/spring-cache-tutorial)

[Testing @Cacheable on Spring Data Repositories | Baeldung](https://www.baeldung.com/spring-data-testing-cacheable)



```java
package example.springdata.jpa.caching;

import org.springframework.cache.annotation.CacheEvict;
import org.springframework.cache.annotation.Cacheable;
import org.springframework.data.repository.CrudRepository;
public interface CachingUserRepository extends CrudRepository<User, Long> {

	@Override
	@CacheEvict(value = "byUsername", key = "#p0.username")
	<S extends User> S save(S entity);

	@Cacheable("byUsername")
	User findByUsername(String username);
}
```



@Override 를 통해 Repository interface 에서도 캐싱이 가능함 



[spring-data-examples/jpa/example/src/main/java/example/springdata/jpa/caching at master · spring-projects/spring-data-examples (github.com)](https://github.com/spring-projects/spring-data-examples/tree/master/jpa/example/src/main/java/example/springdata/jpa/caching) 참고



```java
@AutoConfigureMockMvc
@Tag(INTEGRATION)
@SpringBootTest
class VirusTotalApiKeyScenarioTest {

    @Autowired private MockMvc mockMvc;
    @Autowired private ObjectMapper objectMapper;
    @Autowired private CacheManager cacheManager;

    @AfterEach
    void deleteVirusTotalApiKey() throws Exception {
        mockMvc.perform(delete("/virusTotalApiKeys/{id}", VIRUS_TOTAL_API_KEY)
                .header(API_KEY_AUTH_HEADER_NAME, ADMIN_API_KEY));
    }
    
    private boolean isCacheExists() {
        return ofNullable(cacheManager.getCache(VIRUSTOTAL_API_KEY_CACHE_KEY))
                .map(cache -> cache.get(SimpleKey.EMPTY))
                .isPresent();
    }
}
```

