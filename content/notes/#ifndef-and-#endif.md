---
title: "#ifndef and #endif"
date: 2019-04-06
tags: ["cpp", "c"]
---



 선행 처리기(preprocessor)는 원시 프로그램 내의 모든 선행 처리기 지시자를 선행 처리하여 컴파일러가 컴파일 할 수 있게 C 언어의 문장으로 바꾸어 주는 일을 합니다. 이중에서 #pragma once와 #ifndef ~ #endif 는 헤더파일의 중복 컴파일 방지를 위해 쓰이는 선행 처리기 지시자 입니다. 같은 역할을 하지만 미묘하게 차이가 있고 이 부분에 대해 궁금해 하다가 나름대로 정리를 하게 됬습니다.

  

### 1. #ifndef ~#endif

  

#### first.h

  

```c++
#ifndef __FIRST_H_
#define __FIRST_H__
class First { 
   .... 
}
#endif
```

  

#### second.h

  

```cpp
#ifndef __SECOND_H_
#define __SECOND_H_

#include "First.h"

class Second {
   ....
}

#endif
```

  

#### main.cpp

  

```cpp
#include "First.h"
#include "Second.h"

void main() {
   ...
}
```

  

#### main.cpp를 컴파일 했을 때,

  

```cpp
/* First.h 의 소스코드가 전달됨 by #include "First.h" */

First.h 의 소스코드 

/* Second.h 의 소스코드가 전달됨 by #include "Second.h" */

#ifndef__SECOND_H_
#define __SECOND_H_

#include "First.h"           /* 이 부분에서 중복이 일어남 ! */

class Second {
   ....
}

#endif

/* main 함수의 시작 */ 

main.cpp 의 소스코드

```

  

first.h의 내용이 두번 들어가게 됩니다. 이때 #ifndef ~ #endif 를 통해 중복을 피할 수 있습니다.

==곧 선행 처리기는 First.h를 두번 열어서 확인을 해봐야합니다. #ifndef를 확인하고서야 중복 컴파일을 하지 않을 것입니다.==

  

### 2. #pragma once  

==#pragma once는 헤더파일의 포함횟수에 관계없이 단 한번만 처리를 하고 같은 파일의 경우 파일을 읽기조차 하지 않습니다== 곧, #pragma once가 명시되면 컴파일러가 파일 열기 자체를 시도하지 않습니다. 해당 파일이  몇번 #include 되었던지 간에 단 한반만 컴파일 하게 됩니다. 

  

#### 3. 장/단점 및 결론

#### 장점/단점  

 #pragma once의 경우 컴파일 동안 오직 한번만 포함되도록 하기 때문에 파일을 open 하지도 않습니다. 따라서 #ifndef~ #endif 의 방법보다 컴파일 속도는 빠를 수 있습니다.  

 그러나 #pragma once는 일부 구형 컴파일러에서는 지원이 안되는 경우가 있고, 안정성과 범용성에 있어서는 #ifnedf ~ #endif 를 쓰는 것이 더 좋다고 할 수 있습니다.

  결과적으로는 #pragma once가 더 좋아보이지만 #include의 횟수에 관계 없이 이를 남용하게 되면 소스코드간에 관계가 잘 안보일 수 있고, 중복되는 #include가 많다면 프로그램 전체의 구조가 올바르지 않을 수 있습니다. ==당연하게도 이렇게 저렇게 #include 를 뒤섞은 복잡한 구조는 더 나은 설계가 존재한다는 반증이 되기도 합니다.== 기존의 컴파일러가 이런 번거로운 #ifndef ~ #endif 를 사용하도록 표준화 되었던 것도 이런 원리에 입각한 문법이 아니었나 생각해 봅니다. 

  

### 결론  

 구형 컴파일러에서 동작하는 범용적인 소스를 작성해야 하는 경우라면 #ifndef ~ #endif를 사용해야 하고 이 외에는 ==#pragma once가 사용하기 편하지만, #include 혹은 #pragma once가 남발된 경우 더 나은 설계나 모듈화가 가능한지 점검해볼 필요가 있음==



### 참고  

<http://devmae.tistory.com/315>

<http://neodreamer-dev.tistory.com/310>

<http://www.angel25.com/ClanguageStudy2.html>