# How to make general function container in C++

Category: cpp
Created: November 2, 2020 9:04 PM
Created By: RaeCheol Park
Last Edited Time: November 2, 2020 9:04 PM
Status: DONE

# **INTRO**

임의의 모든 Windows API 함수를 후킹하는 방법을 찾아보다가 모든 함수를 담는 container 가 있으면 일일이 후킹하는 코드를 작성하는 것이 아니라 간단하게 구현할 수 있지 않을까 생각해 봤었다. 처음에는 이런게 가능은 한가 싶었는데 찾아보니까 가능은 하더라 ..
결과적으로는 이런 방법을 사용해서 일반적인 후킹을 구현하는 방법을 찾지는 못했는데 구현된 코드의 모양이 재미있기도 하고, 혹시 나중에 쓸 일이 있을까 싶어 이렇게 기록해 둔다. cuckcoo sandbox의 경우 후킹의 대상이 되는 함수들의 목록을 저장해두는 파일이 있다. 인자의 수, 타입, 반환형들의 나열이 있는데 해당 파일을 읽어 후킹 하는 코드를 작성하는 식으로 구현되어 있는 듯 하다.

# **CONTENTS**

## **C언어의 function pointer를 활용하는 방법**

```cpp
 #include <stdio.h>
 void print_number(int arg)
 {
     printf("argument is: %d\n", arg);
 }
  
 int main()
 {
     void (*f)(int);   // f is a variable that can 
                       // store a pointer to a 
                       // function that receives 
                       // an int an returns nothing.
     f = print_number; // legal.
  
     f(1024);          // calls print_number.  
  
     return 0;
 }
```

C언어의 함수 포인터는 되게 못생겨서.. 별로 쓰고싶지는 않은 코드다.

- 가독성이 별로다.
- 포인터를 사용한다는 위험

여러 종류의 포인터를 한 곳에 저장해 둘 수 없다는 것도 단점이다. 모든 포인터를 `void*` 의 형태로 담고 있다가 나중에 형변환을 해야하는데 어떤 형으로 변환할지 저장하는 등 .. 지저분한 과정을 거쳐야만 한다.

## **Generic function wrapper**

### **using class**

```cpp
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

### **usage**

```cpp
 #include <stdio.h>
 /////////////////////////////////////////////////////////////////////////////////////////
 void function_with_one_argument(int arg)
 {
     printf("hello with argument %d", arg);
 }
 
 /////////////////////////////////////////////////////////////////////////////////////////
 void function_with_two_argument(int arg1, int arg2)
 {
     printf("hello with argument %d and %d", arg1, arg2);
 }
 
 /////////////////////////////////////////////////////////////////////////////////////////
 int main (void) 
 {
     generic::function<void, int> gf(function_with_one_argument);
     generic::function<void, int, int> gf2(function_with_two_argument);
     gf(1);
     gf2(1, 2);
 }
```

- 함수를 정의하는 부분은 아주 깔끔하다.
- 대신 사용하는 사람이 조금 더럽게 사용해야한다.
- 사실 가독성 측면에서 C 에서의 함수 포인터와 크게 차이가 없는 것 처럼 느껴진다.

## **using variadic template function**

```cpp
 template<typename Fn, Fn fn, typename... Args>
 typename std::result_of<Fn(Args...)>::type
 wrapper(Args&&... args) {
     return fn(std::forward<Args>(args)...);
 }
 #define WRAPPER(FUNC) wrapper<decltype(&FUNC), &FUNC>
```

### **usage**

```cpp
 #include<iostream>
 #include<cstdlib>
 /////////////////////////////////////////////////////////////////////////////////////////
 int min(int a, int b){
     return (a < b) ? a : b ;
 }
 
 /////////////////////////////////////////////////////////////////////////////////////////
 int main(void){
     std::cout<<WRAPPER(min)(10, 20)<<'\n';
     std::cout<<WRAPPER(rand)()<<'\n';
 }
```

- template 으로 정의하는 부분 자체가 너무 복잡해서 가독성이 떨어진다.
- 다만 사용자 입장에서는 내부 구현을 신경쓰지 않는다면, 조금더 깔끔하게 사용할 수 있다.

## **그래서?**

이렇게 저렇게 해서 무슨 API 함수든지 다 담을 수 있게 만들었지만 담은 후에 실제로 후킹을 하는 코드를 짜보면 이것만으로는 여전히 부족하다는 것을 알 수 있다. 더 삽질을 해보면 더 좋은 방법이 있을 것 같기도 한데.. 잘 안된다..

# **Referenced**

[Wrap a function pointer in C++ with variadic template](https://stackoverflow.com/questions/25392935/wrap-a-function-pointer-in-c-with-variadic-template)

[How to GetProcAddress() like a boss](https://blog.benoitblanchon.fr/getprocaddress-like-a-boss/)