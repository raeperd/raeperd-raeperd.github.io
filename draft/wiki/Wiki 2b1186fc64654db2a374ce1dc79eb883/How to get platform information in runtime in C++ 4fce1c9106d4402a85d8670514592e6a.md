# How to get platform information in runtime in C++

Category: cpp
Created: November 2, 2020 9:04 PM
Created By: RaeCheol Park
Last Edited Time: November 2, 2020 9:04 PM
Status: DONE

크로스 플랫폼을 지원하는 cpp 프로젝트에서 platform specific 한 코드를 빌드하기 위해 매크로를 많이 사용한다. 보통 이런식

```cpp
#ifdef _WIN32 // note the underscore: without it, it's not msdn official!
    // Windows (x64 and x86)
#elif __unix__ // all unices, not all compilers
    // Unix
#elif __linux__
    // linux
#elif __APPLE__
    // Mac OS, not sure if this is covered by __posix__ and/or __unix__ though...
#endif
```

`_MSC_VER` 를 통해 윈도우를 구분해 낼 수도 있지만 이는 플랫폼이 아니라 컴파일러만을 구분해낸다. 윈도우에서 다른 컴파일러로 컴파일한다면 확인되지 못함

컴파일 타임이 아니라 런타임에 실행되고 있는 환경을 감지하려면 어떻게 해야할까? 가령 윈도우 MSVC에서 리눅스용 바이너리를 빌드한다면, 매크로는 윈도우지만 런타임은 리눅스가 될 것이다.

리눅스인지 확인하는 건 루트 디렉토리가 디렉토리로 존재하는지? 같은 쉘 커맨드를 실행해보는 방법은 물론 가능하지만 더 간단하게도 가능하다.

```cpp
if (sizeof(wchar_t) == 4)
    printf("linux");
if (sizeof(wchar_t) == 2)
    printf("windwos");
```

커맨드를 실행하거나 하는 거 없이 알아낼 수 있다. 리눅스와 맥의 구분은 커맨드 실행 말고는 딱히 방법이 떠오르지는 않는다..?