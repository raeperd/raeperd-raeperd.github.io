# Read test resource in junit5

Category: java
Created: November 25, 2020 3:01 PM
Created By: RaeCheol Park
Last Edited Time: November 25, 2020 3:01 PM
Status: NEED_DETAIL

```java
private String readSampleReportContents() throws IOException {
        return ofNullable(getClass().getClassLoader().getResourceAsStream("malicious.json"))
                .map(Scanner::new)
                .map(scanner -> scanner.useDelimiter("\\A").next())
                .orElseThrow(FileNotFoundException::new);
    }
```