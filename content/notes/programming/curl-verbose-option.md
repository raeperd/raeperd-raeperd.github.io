---
title: "Curl Verbose Option"
date: 2021-03-28
tags: ["bug"]
---

같은 명령인데 CentOS에서는 되고 우분투에서는 안되는 문제가 발생

```cpp
curl --location --request POST 'https://api.ai-gsp.com/tidb/hash' \
--form 'api-key="146ae893-944a-407c-adfe-9ad14ace212d"' \
--form 'timestamp="2021-03-01 11:00:00"'
```

form 을 전달할때 `"` 를 포함했을때, CentOS에서는 정상 동작했고, Ubuntu에서는 동작을 안했다.

`"` 을 제거하니 우분투에서 정상동작 확인

Cent 에서도 마찬가지로 정상동작 됨 

<mark>
curl 요청에 form 을 포함할때는 `"` 를 포함하지 않아야 한다.
</mark>