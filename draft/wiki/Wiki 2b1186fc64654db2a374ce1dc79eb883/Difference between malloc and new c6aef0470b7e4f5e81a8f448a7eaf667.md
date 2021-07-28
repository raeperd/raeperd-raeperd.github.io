# Difference between malloc and new

Category: cpp
Created: November 2, 2020 8:54 PM
Created By: RaeCheol Park
Last Edited Time: November 2, 2020 8:54 PM
Status: DONE

# **INTRO**

C에서의 동적할당을 생각해보자

- `malloc` 이 용도를 생각하지는 않는다. 좋게 말하면 자유도가 높은 것이지만 다르게 생각해보면 안전장치가 없다는 것.
- `malloc` 은 옛날 패러다임에 맞춰진 함수다. 이전의 환경과 달리 현대의 컴퓨터들은 RAM이 커지고 OS가 더 똑똑해졌다.
    - 메모리 => 가상메모리
    - Ring과 같은 권한 부여 시스템
    - OS가 메모리를 통제함으로써 할당 실패 가능성이 줄어듬

C++ 에서는 동적할당을 `new` 로 한다. 당연히 `new` 는 `malloc` 이후의 문법이기 때문에 이런저런 단점을 극복하려 애를 썼을 것이다. 어떤 차이가 있는지 대충 감으로만 알고 있다가 확실하게 공부를 해봤다.

# **CONTENTS**

## **문법**

### **할당과 해제**

```cpp
 // C style 동적 할당
 int *pnData = (*int)malloc(sizeof(int)*5);
 free(pnData);
 
 // C++ style 동적 할당
 int *pnNewData    = new int;
 int *pnNewDataArr = new int[5];
 
 delete pnNewData;
 delete [] pnNewData;
```

- C style 은 직관적이지 못하고 문법이 지저분해보인다.
- 할당한 메모리를 해제할때, `delete` 의 경우 배열로 할당한 메모리는 배열로 지워야한다.
- `new` 로 동적할당할 경우의 반환형은 주소값이다.

### **생성자 호출 여부**

```cpp
 #include <iostream>
 using namespace std;
 
 class CTest{
     int a;
 
 public:
     CTest(void)
     : a(0){
         cout << __FUNCTION__ << endl;
     }
 
     ~CTest(void){
         cout << __FUNCTION__ << endl;
     }
 };
 
 int main(){
     CTest test;                 // 생성자 >>>> 소멸자
     CTest* pTest1 = (CTest*)malloc(sizeof(CTest));
     CTest* pTest2 = new CTest();// 생성자
     delete pTest1;              // 소멸자
     delete pTest2;          
     return 0;
 }
```

- `malloc` 은 생성자를 호출하지 않는다.
- `new` 는 생성자를, `delete`는 소멸자를 호출한다.
- 반면에 `malloc` 과 `free` 는 그렇지 않다.

### **예외 상황에서의 return 값**

`malloc` 은 메모리 할당에 실패할 경우 `NULL` 을 return 한다. 그래서 실제로 `malloc` 을 사용하는 코드에서도 매번 반환되는 값이 `NULL` 인지 확인하는 코드가 필요하다.

`new` 의 경우 `std::bad_alloc` 의 예외를 던진다. 예외를 던지기 때문에 `new`를 사용하는 코드에서는 따로 추가적인 확인이 필요없겠지만 어디에선가는 분명 `catch` 를 통해 처리를 해줘야 한다. 예외가 발생하는 부분과 처리하는 부분의 코드가 물리적으로 떨어져 있을 수도 있기 때문에 이런 불규칙적인 흐름변화가 코드의 가독성을 떨어뜨릴 수 있다.

++)

`**std::nothow` 를 이용하면 예의를 던지지 않도록 설정할 수도 있다.** 

### **override**

`new` 의 경우 사용자가 정의하는 객체에서 override 를 할 수도 있다. `new` 는 operator 이며 `malloc` 은 함수다. (당연히 operator 도 함수의 일부긴 하다.)

### **재할당**

`malloc` 을 사용할 경우 이미 할당했던 메모리를 다시 활용할 수 있다. (물론 이게 많은 취약점의 근원이 된다.) 반면에 `new` 는 그렇지 못하다.

## **요약**

1. `malloc` 의 문법이 `new` 보다 지저분하다.
2. `malloc` 은 생성자를 호출하지 않고 ,`new` 는 호출한다.
3. `malloc` 은 실패하면 `NULL` 을 리턴하고, `new` 는 예외를 던진다.
4. `malloc` 은 재할당을 할 수 있고, `new` 는 객체에서 override 를 할 수 있다.

여기까지만 보면 굳이 `malloc` 을 사용할 이유가 없어보인다. 그렇지만 C++ 에서는 하위호환성을 위해서인지 ? 아니면 생성자를 호출하지 않고 인스턴스를 생성할 필요가 있다고 생각해서인지? `malloc` 과 `free` 의 호출을 허용한다.

중요한점은 허용한다고 해서 써도 된다는게 아니라는 점. 문법이 이를 허용하더라도 그냥 사용하지 말아야 한다.

바로 위의 생성자와 소멸자와 연관된 부분인데, 따로 정의된 객체를 `malloc` 으로 할당할 경우 생성자가 호출되지 않기 때문에 객체를 정의한 프로그래머의 의도와 예상을 빗나가는 로직이 실행될 가능성이 매우 크다. 물론 생성자와 소멸자에 아무런 로직이 없다면 문제가 발생하지 않을 것 같지만, 굳이 생성자와 소멸자 코드를 확인해야한다는 점에서 이미 활용할 가치가 없어보인다.

또 아주 많은 Heap 영역에서의 취약점이 malloc 의 type casting 으로 인해 생길 수도 있다. 기본적으로 `malloc`의 반환값은 `void*` 이기 때문에 이를 변환해줘야 하고 항상 잘못 사용할 가능성을 내포하고 있다는 점에서도 `malloc` 을 쓸 이유가 없다. 적어도 C++ 에서는 그렇다. C에서는 당연하게도 `malloc` 을 사용할 수 밖에 없다.

그래도 혹시 모른다. `malloc` 이여만 하는 경우가 있기 때문에 문법적으로 허용해 둔 것이 아닐까 ? 내공이 부족해서 그런가 아직은 잘 모르겠다.

# **Referenced**

[StackOverFlow - in what case do i use malloc or new](https://stackoverflow.com/questions/184537/in-what-cases-do-i-use-malloc-and-or-new%20)