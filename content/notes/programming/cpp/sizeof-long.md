---
title: "Sizeof Long"
date: 2019-12-21
tags: ["cpp"]
---

# `long` 의 크기
```cpp
 #define SHOW_SIZEOF(x) printf("sizeof(#x) is %u\n", sizeof(x))
 
 int main(int argc, char* argv)
 {
     printf("Pointer size is %u \n", sizeof(void*));
     SHOW_SIZEOF(char);
     SHOW_SIZEOF(short);
     SHOW_SIZEOF(int);
     SHOW_SIZEOF(long);
     SHOW_SIZEOF(long long);
     SHOW_SIZEOF(float);
     SHOW_SIZEOF(double);
     return 0;
 }
```

64비트 윈도우에서 `long` 은 4바이트 지만 리눅스와 맥에서는 8바이트다.
cpp 표준에서 long 은 주소를 표현하는 단위의 크기로 규정하고 있지만,
`DWORD`의 하위 호환성을 위해 윈도우에서는 `long` 을 8바이트로 사용하지 못하는 것 같다.
크로스 플랫폼으로 개발한다면, long 변수의 사용은 자제해야한다.
