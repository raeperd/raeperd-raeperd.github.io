---
title: "No Converter Found for Return Value of Type"
date: 2020-11-20
tags: ["java", "spring", "bug"]
---

RestController 의 메소드에서 인자와 반환값을 json 으로 Serialize / Desiriallize 가 불가능할 때 나타나는 에러다.

Getter와 생성자만 적절하게 존재하면 가능하다

대충 이런식

```java
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.AllArgsConstructor;

@Getter
@NoArgsConstructor
@AllArgsConstructor
public class ScanRequest {

    private String url;

}
```

@RequestBody 가 json Serializable 하려면 NoArgsConstructor 가 필요하다.

테스트를 위해서는 @AllArgConstructor도 필요할 것 프래임 워크가 가져와주는 것이라 필드들을 열어놔야 한다. 

그런데 Controller 의 Return 이 되는 경우에는 생성자를 제공할 필요가 없다. Getter만 있으면 된다.

아래의 Result 도 정상적으로 동작한다. 

```java
import lombok.Getter;

@Getter
public class ScanResult {

    private final String scan_result;

    private ScanResult(String scan_result) {
        this.scan_result = scan_result;
    }

    public static ScanResult Benign() {
        return new ScanResult("benign");
    }

}
```

테스트는 이런식으로 할 수 있다.

```java
import com.fasterxml.jackson.databind.ObjectMapper;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.http.MediaType;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.request.MockMvcRequestBuilders;

import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;

@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.MOCK)
@AutoConfigureMockMvc
class ScanRestControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @Autowired
    private ObjectMapper objectMapper;

    @Test
    public void postScanTest() throws Exception {
        ScanRequest scanRequest = new ScanRequest("url");
        String scanRequestJson = objectMapper.writeValueAsString(scanRequest);

        mockMvc.perform(
                MockMvcRequestBuilders.post("/api/scan")
                .contentType(MediaType.APPLICATION_JSON)
                .content(scanRequestJson)
                .accept(MediaType.APPLICATION_JSON))
                .andExpect(status().isOk())
                .andExpect(content().contentType("application/json"))
                .andExpect(jsonPath("$.success").value(true))
                .andExpect(jsonPath("$.response.scan_result").value("benign"));
    }
}
```

[java.lang.IllegalArgumentException: No converter found for return value of type](https://stackoverflow.com/questions/37841373/java-lang-illegalargumentexception-no-converter-found-for-return-value-of-type)

The problem was that one of the nested objects in Foo didn't have any getter/setter

**it needs only getters**