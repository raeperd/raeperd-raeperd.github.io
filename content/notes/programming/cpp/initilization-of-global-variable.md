---
title: "Initilization of Global Variable"
date: 2020-07-21
tags: ["cpp", "bug"]
---

저번주에 꽤나 발견하기 어려웠던 오류 하나를 발견했는데, 전역변수와 관련된 버그였다. 결론은, *전역변수의 초기화는 변수간의 의존성에 영향을 받지 않는다*는 것

예를 들어, `g_fRadius` 라는 전역변수가 `g_fPie` 라는 변수에 의존적이고, 서로 다른 파일에서 초기화가 된다고 생각해보자.

C++ 컴파일을 하는 과정 중에 .h .cpp 파일은 include 를 통해서 일렬로 정렬되는 과정이 포함된다. 전역 변수의 초기화 시점은 이 과정에 영향을 받아, 먼저 include 되는 파일의 전역변수가 먼저 초기화 된다.

`g_fRadius` 라는 전역변수가 정의하려는 순간에, `g_fPie` 는 적절한 `include`와 `extern` 을 통해 참조할 수 있다. 그러나 참조하는 것은 나라는 개발자가 참조하는 것. 내가 코드를 작성하는 그 순간에는 어떤 변수도 초기화 되지 않았다. 변수가 초기화되는건 컴파일러가 어셈블리 코드를 *작성*할떄다. 어떤 순서로 작성하느냐에 따라, `g_fRadius` 는 어떤 값으로 초기화 되기전인, 기본 값인 0의 `g_fPie`를 참조할 수도 있고, 3.14f 의 값을 참조할 수도 있다. 내가 참조하는 순서를, 컴파일러가 고려해주지 않는다.

java나 pytho, go 같은 더 고급진 언어들은 이런것들고 고려해서 초기화를 해줄지 잘 모르겠다. include 시스템이 아닌 import 를 사용하는 패키징 시스템에서는 뭔가 다른 요소가 순서에 영향을 줄 수 도 있다.

예상을 확인해보려고 간단하게 프로젝트를 만들어봤는데 대략 테스트코드만 봐도 어떤 테스트를 했는지 알 수 있을 것이다. 잘 모르겠으면 전체 gist를 보면 됨

[https://gist.github.com/raeperd/ffc56285ea4ec2156f2d5892cabc4d0a#file-abxtest-cpp](https://gist.github.com/raeperd/ffc56285ea4ec2156f2d5892cabc4d0a#file-abxtest-cpp)

[ms docs keyword extern](https://docs.microsoft.com/ko-kr/cpp/cpp/extern-cpp?view=vs-2019)

[Why does const imply internal linkage in C++, when it doesn't in C?](https://stackoverflow.com/questions/998425/why-does-const-imply-internal-linkage-in-c-when-it-doesnt-in-c)

[C++ global extern constant defined at runtime available across multiple source files](https://stackoverflow.com/questions/26072216/c-global-extern-constant-defined-at-runtime-available-across-multiple-source-f)

[C++ global initialization order ignores dependencies?](