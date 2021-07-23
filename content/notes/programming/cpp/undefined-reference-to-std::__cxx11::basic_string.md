---
title: "Undefined Reference to Std::__cxx11::basic_string"
date: 2019-12-21
tags: ["cpp", "bug"]
---

g++ 4.8.1

```
 [root@03b3922d2ae8 macropredictor]# make 
 [  8%] Built target ConnectorLib
 [ 10%] Linking CXX executable ../../Build/x64LinuxRelease/MacroServer
 CMakeFiles/MacroServer.dir/CGraph.cpp.o: In function `CGraph::CGraph(CGraph const&)':
 CGraph.cpp:(.text+0x50d): undefined reference to `core::MBSFromMBS(std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> >)'
```

C++11 이전의 컴파일러로 빌드된 라이브러리를 최신의 프로그램에서 링크할때 발생하는 오류. 특히 리눅스 환경의 gcc 컴파일러를 사용할때 이런 오류를 발견할 수 있는데 g++ 4.8.1 전후로 std::string 의 구현체의 인터페이스가 변경되었기 때문이다.

C++11 전후로 C++ 이 엄청 많이 바뀌었기 때문에 gcc 컴파일러 개발자들도 과감하게 인터페이스를 바꿀 수 있었던게 아닐까 싶지만 하위 호환성을 유지해야하는 입장에서는 좀 지옥같은 버그다.

새로운 프로젝트에서 `_GLIBCXX_USE_CXX11_ABI` 매크로를 0으로 설정하는 것으로 이전 인터페이스의 std::string 을 사용함을 명시할 수 있고 링크 오류는 해결 할 수 있다. 

좋은 cmake 파일은 아니지만 아래처럼 설정할 수도 있다. 

```
 if(CMAKE_COMPILER_IS_GNUCXX)
     add_definitions(-std=c++11)
     add_definitions(-D_GLIBCXX_USE_CXX11_ABI=0)
 endif()
```

사실 최선은 하위 호환성을 버리고 새로운 빌드 산출물에만 신경을 쓰는게 좋은데 C++11 이전 이라 함은 이제 10년이 다되가는 소프트웨어다. 이런 프로그래밍을 계속 해야하나 싶다 진짜