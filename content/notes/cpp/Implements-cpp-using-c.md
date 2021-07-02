---
date: 2019-10-25
draft: true
tags: ["cpp"]
---

# Motivation 

 프로그래밍을 배우면 자연스럽게 **객체지향**(OOP: Object Oriented Programming)이라는 말을 듣게 된다. 필요하고 중요한 개념이지만 학교에서 처음 객체지향에 대해 알게 되었을때는 그 필요성이 와닿지 않았다. 실무에서 쓰이는 코드를 만들다보니 이제는 좀 와닿는 것 같다. 그런데 누군가 나에게 객체지향이 왜 필요하냐고 물어보면 또 잘 설명할 자신이 없었다. 객체지향의 필요성을 강조하는 자료는 구글링을 조금만 해봐도 많이 나오지만, 대부분 객체지향을 사용하는 사람들이야 공감하는 내용이지 프로그래밍 경험이 적은 사람들이 쉽게 와닿게 잘 설명된 자료는 없는 것 같다. 이럴때 C에서 C++ 로 넘어가는 과정을 이해하면 좀 더 객체지향에 대해 더 잘 이해할 수 있다. C++ 강의를 참 많이 들어봤지만 직접 C++ 의 클래스의 일부를 구현해보는 것 만큼 와닿는 설명이나 예제는 없는 것 같다.


# Contents
## Before OOP

대부분의 경우에 프로그램의 핵심 논리를 구현하는 사람과 사용하는 사람은 다르다. USERDATA 라는 구조체를 활용해서 이름이랑 나이를 저장하는 간단한 프로그램을 만든다고 할때, 기존의 C, 절차지향 프로그래밍을 사용한 프로그램은 대략 아래와 같은 방식으로 구현할 수 있다.

``` c
 #include <stdio.h>
 
 // 제작자의 코드
 typedef struct USERDATA {
     int nAge;
     char szName[32];
 } USERDATA;
 
 // 사용자의 코드
 int main(void) {
     USERDATA x = {20, "철수"};
     printf("%d, %s \n", x.nAge, x.szName);
 
     return 0;
 }
```

이렇게 구현했을 때 생기는 문제점이 몇가지 있다.
* 사용자는 제작자가 만든 자료구조(구조체)의 멤버 및 구성을 알고 있어야 한다.
* 제작자가 자료구조를 변경한다면, 사용자는 코드와 관련된 자신의 코드를 모두 수정해야 한다.
* **즉, 제작자는 편하지만 사용자는 불편한 상황이 발생한다.**
이런 문제점들을 아래와 같이 개선할 수 있다.

## Toward OOP

### 첫번째 개선 - sturct as parameter

``` c
 #include <stdio.h>
 
 // 제작자의 코드
 typedef struct USERDATA {
     int nAge;
     char szName[32];
 } USERDATA;
 
 void PrintData(USERDATA *pUser) {
     printf("%d, %s \n", pUser->nAge, pUser->szName);
 }
 
 // 사용자의 코드
 int main(void) {
     USERDATA x = {20, "철수"};
     PrintData(&x);
     return 0;
 }
```

이제 제작자가 구조체를 변경하더라도 사용자는 출력하는 코드를 변경할 필요가 없다. 구체적으로 두가지 장점이 생겼다.
1. PrintData() 함수를 사용자에게 제공함으로써 출력을 위한 구체적인 방법을 사용자가 알 필요가 없어졌다.
2. 사용자는 이제 함수를 제공받아 사용하면 되며, 함수의 구체적인 구조까지 알아야 할 필요가 없다.
곧 사용자는 함수의 사용방법 (입력이 무엇이고 출력이 무엇인지) 에 대해서만 알면 되며, 이런 역할을 하는 함수를 인터페이스 함수라고 한다.
그런데 여전히 불편한 점이 한가지 있다.
* 자료구조(USERDATA) 와 함수(PrintData)가 따로 존재한다.
* 자료구조와 함수가 관계가 있다는 것을 확인하기 불편하다.
지금은 자료구조와 함수의 정의가 물리적으로 가까이 붙어 있지만, 프로그램이 커지고 복잡해질 수록 자료구조와 함수를 함께 보기 어려워 질 것이다. 자료구조와 함수가 같이 붙어다니면 좋을 것 같다.

  

### 두번째 개선 - function pointer

``` c
 #include <stdio.h>
 
 // 제작자의 코드
 typedef struct USERDATA {
     int nAge;
     char szName[32];
     void(*Print)(struct USERDATA*);
 } USERDATA; 
 
 void PrintData(USERDATA *pUser) {
     printf("%d, %s \n", pUser->nAge, pUser->szName);
 }
 
 // 사용자의 코드
 int main(void) {
     USERDATA user = {20, "철수", PrintData};
     user.Print(&user);
     return 0;
 }
```

 이제 사용자는 USERDATA 를 출력하는 함수를 찾아다닐 필요가 없다. USERDATA가 PrintData 함수를 알고 있어야 한다는 사실은 불편하지만 (다른 파일에 존재할 경우 따로 include 를 해야한다는 점에서) 사용자는 편하게 사용할 수 있다.

다만 아직 한가지 꺼림직한 점이 있는데, 사용하는 입장에서 user.Print(&user) 라는 코드의 어색함이다. user 가 가지고 있는 함수 Print 를 사용하는데 왜 다시 user 를 넘겨줘야하는가?

C 에서는 이를 문법적으로 해결할 방법이 없다. (적어도 내가 알기로는 그렇다) 이를 C++ 에서 문법적으로 해결했다.

 

### 세번째 개선 - class

``` cpp
 #incldue <iostream>
 using namespace std;
 
 // 제작자 코드
 class USERDATA {
 public:
     void Print(void) {
         cout << nAge << ", " << szName << endl; 
     }
 
 private:
     int nAge;
     char szName[32];
 };
 
 // 사용자 코드
 int main(void) {
     USERDATA user = {20, "철수" };
     user.Print();
 
      return 0;
 }
 
```

이제 클래스를 사용하는 사람은 편하게 사용할 수 있다. 단순히 클래스의 멤버 Print() 함수를 호출하면 데이터를 출력할 수 있다는 사실만 알고있으면 된다. 자료구조와 알고리즘은 이제 프로그램이 얼마나 더 복잡해지더라도 같은 파일, 같은 공간에서 확인할 수 있다.
객체지향을 사용해야하는 또 다른 수 많은 이유가 있겠지만, 객체지향을 몰랐던 사람이 그 필요성을 느끼게 되는데는 꽤나 적절한 예제라고 생각한다. 이 예제는 이것이 C++이다 라는 책을 통해 봤는데 작가분이 설명을 너무 잘해주신 것 같다.  [동영상 강의](notion://www.notion.so/raeperd/%20https://www.youtube.com/watch?v=kq6b5zBkQ28&list=PLXvgR_grOs1DFOWF65X0Zqnd_264x41u-%20) 로도 이 내용을 확인할 수 있다.



# Reference
   [제 01장 C와는 다른 C++-첫 번째](https://www.youtube.com/watch?v=kq6b5zBkQ28&list=PLXvgR_grOs1DFOWF65X0Zqnd_264x41u-)  [이것이 C++이다](http://www.yes24.com/Product/Goods/23840775) 
