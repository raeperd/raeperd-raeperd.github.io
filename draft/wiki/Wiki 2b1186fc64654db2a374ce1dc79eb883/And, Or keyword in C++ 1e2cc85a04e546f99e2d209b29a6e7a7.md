# And, Or keyword in C++

Category: cpp
Created: November 2, 2020 9:05 PM
Created By: RaeCheol Park
Last Edited Time: November 2, 2020 9:05 PM
Status: DONE

코딩 스타일에 정답은 없다지만, 아주아주 정답처럼 보이는 경우가 몇가지 있다. 이번에 알게됬는데 이건 그냥 정답인거 같아서 논란의 여지도 없어보인다.

C++은 유독 다른 프로그래밍 언어보다 가독성이 떨어져 보인다. 여러 헤더파일이 복잡하게 꼬여있는 구조나, 직관적이지 못한 namespace 등 여러가지 원인이 있겠지만 문법적으로 잘 안보이는 부분이 너무 많다. 대표적인 예제는 `&&` `||` `!` 이다. 특히 `!`

```cpp
bool isPrime(int x);

int main(void)
{
    const int x = 10;
    const int y = 20;
    if ( x == 10 && y == 20 )
    {
        // do something
    }
    if ( x == 10 || y == 20 )
    {
        // do other
    }

    if ( !isPrime(10) )
    {
        // not prime
    }

    return 0;
}
```

`!` 는 정말 잘 안보인다. 이걸 간단하게 해결할 수 있는데 이걸 이제 알았다니 ;;

```cpp
#include <ciso646>
bool isPrime(int x);

int main(void)
{
    const int x = 10;
    const int y = 20;
    if ( x == 10 and y == 20 )
    {
        // do something
    }
    if ( x == 10 or y == 20 )
    {
        // do other
    }

    if ( not isPrime(10) )
    {
        // not prime
    }

    return 0;
}
```

심지어 저 헤더를 포함해야만 하는 줄 알았는데 이 문법은 이미 C++ 의 표준 문법이었다.

C 언어에서 사용하려면 include 가 필요하다.

또 Visual studio 에서는 기본적으로 이걸 제공하지 않는데, /permissive 옵션을 주면 가능하다. 

아마 하위호환성을 위해 막아둔 것으로 보인다.

자바는 이런게 왜 안되나 싶다 

### Thanks to

[cppreference ciso646](https://en.cppreference.com/w/cpp/header/ciso646)

[cppreference Alternative operator representation](https://en.cppreference.com/w/cpp/language/operator_alternative)