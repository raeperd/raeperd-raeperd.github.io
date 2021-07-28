# static const vs constexpr

Category: cpp
Created: November 2, 2020 9:09 PM
Created By: RaeCheol Park
Last Edited Time: November 2, 2020 9:09 PM
Status: DONE

`static const` 와 `constexpr` 의 차이

`static const`의 경우 컴파일타임에 값을 결정하지 못할 경우 런타임에 값이 결정된다.

`constexpr` 는 컴파일 타임에 값을 결정하지 못할 경우 에러를 낸다.

컴파일타임 상수에 대해서는 `constexpr`, run time 상수에 대해서는 `static const` 를 사용하면 의도를 좀 더 명확하게 전달할 수 있다.