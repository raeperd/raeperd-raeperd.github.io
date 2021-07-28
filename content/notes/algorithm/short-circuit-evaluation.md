---
title: Short Circuit Evaluation
date: 2021-07-24
tags: ["algorithm", "bug"]
---

## Short Circuit Evaluation
* 어떤 프로그래밍 언어에서  `AND` 혹은 `OR` 의 연산에 있어서 앞선 연산만으로 전체 연산의 결과를 유추할 수 있을때, 뒤의 연산을 수행하지 않는 연산 방식
* C++, JAVA, Kotlin, Python 등 대부분의 언어에서 지원함 
* [Short-circuit evaluation - Wikipedia](https://en.wikipedia.org/wiki/Short-circuit_evaluation)

 
### AND 
* `AND` 연산의 경우에 `false` 가 우선 나와버리면 `AND` 뒤에 나오는 연산은 생략.

### OR 
* `OR` 연산의 경우에 `true` 가 우선 나와버리면 `OR` 뒤에 나오는 연산은 생략.

## Example
### AND
``` cpp
#include <stdio.h>

int main (){ 
	int pass = 0; 
	int i = 0; 
	if(pass && i++){} //i++이 실행되지 않습니다. 
	printf("pass: %d i: %d\n", pass, i); return 0;
}
```

``` shell
$ ./and_example
$ pass: 0 i: 0
```

### OR 
``` cpp
#include <stdio.h>

int main (){ 
	int pass = 1; 
	int i = 0; 
	if(pass || i++){} //i++이 실행되지 않습니다. 
	printf("pass: %d i: %d\n", pass, i); 
	return 0;
}
```

결과는 아래와 같습니다.
``` shell
$ ./or_example
$ pass: 1 i: 0
```

# Reference
- [[Algorithm] Short Circuit Evaluation이란?](https://twpower.github.io/53-about-short-circuit-evaluation)

