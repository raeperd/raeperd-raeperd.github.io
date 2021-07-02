---
title: "function container for hooking engine"
date: 2020-01-13
tags: ["cpp"]
---

# Motivation

임의의 모든 Windows API 함수를 후킹하는 방법을 찾아보다가 모든 함수를 담는 container 가 있으면 일일이 후킹하는 코드를 작성하는 것이 아니라 간단하게 구현할 수 있지 않을까 생각해 봤었다.

처음에는 이런게 가능은 한가 싶었는데 찾아보니까 가능은 하더라 .. 

결과적으로는 이런 방법을 사용해서 구현하는 방법을 찾지는 못했는데 구현된 코드의 모양이 재미있기도 하고, 혹시 나중에 쓸 일이 있을까 싶어 이렇게 메모해 둠. 

혹시 이 방법으로 함수 후킹을 성공하신 분이 있으시다면 저에게도 꼭 알려주셨으면 합니다. ㅎㅎ 

<br>

# Contents
## Function pointer in C

```C++
#include <stdio.h>
void print_number(int number)
{
    printf("Argument is: %d\n", number);
}
 
int main(int argc, char* argv[])
{
    void (*f)(int);   // f is a variable that can 
                      // store a pointer to a 
                      // function that receives 
                      // an int an returns nothing.
    f = print_number; // legal.
 
    f(1024);          // calls print_number.  
 
    return 0;
}
```

- 가독성이 별로다.
- 포인터를 사용한다는 위험
- 여러 종류의 함수를 한 곳에 저장해 둘 수 없다는 것도 단점! 이게 불가능 하다면 사실상 의미가 없

<br>

## Generic function wrapper

---

### using class

```C++
namespace generic {
    template<typename R, typename... T>
    class function {
        public:
            function(R *(f)(T...)) { _f = f; }
            R operator()(T... args) {return _f(args...);}
        
        private:
            R (*_f)(T...);
    };
}
```

### usage

```C++
#include <iostream>
void testfunc(int arg) {
    std::cout << "hello (arg = " 
              << arg
              << ")"
              << std::endl;
}

int main (int argc, char* argv[]) {
    generic::function<void, int> gf(testfunc);
    gf(909);
}
```

- 함수를 정의하는 부분은 아주 깔끔하다.
- 대신 사용하는 사람이 조금 더럽게 사용해야한다.
- 사실 가독성 측면에서 C 에서의 함수 포인터와 크게 차이가 없는 것 처럼 느껴진다.

<br>

### using variadic template function

```C++
template<typename Fn, Fn fn, typename... Args>
typename std::result_of<Fn(Args...)>::type
wrapper(Args&&... args) {
    return fn(std::forward<Args>(args)...);
}
#define WRAPPER(FUNC) wrapper<decltype(&FUNC), &FUNC>
```

<br>

### usage

```C++
int min(int a, int b){
    return (a<b)?a:b;
}

#include<iostream>
#include<cstdlib>
int main(){
    std::cout<<WRAPPER(min)(10, 20)<<'\n';
    std::cout<<WRAPPER(rand)()<<'\n';
}
```

- template 으로 정의하는 부분 자체가 너무 복잡해서 가독성이 떨어진다.
- 다만 사용자 입장에서는 내부 구현을 신경쓰지 않는다면, 조금더 깔끔하게 사용할 수 있다.

### Referenced

[Wrap a function pointer in C++ with variadic template](https://stackoverflow.com/questions/25392935/wrap-a-function-pointer-in-c-with-variadic-template)