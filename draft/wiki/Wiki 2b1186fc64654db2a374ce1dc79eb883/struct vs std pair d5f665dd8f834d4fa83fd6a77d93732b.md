# struct vs std::pair

Category: cpp
Created: November 2, 2020 9:10 PM
Created By: RaeCheol Park
Last Edited Time: November 2, 2020 9:10 PM
Status: DONE

[struct vs std::pair](https://stackoverflow.com/questions/2236182/what-is-the-difference-between-using-a-struct-with-two-fields-and-a-pair)

`std::pair` 는 여러 operator 들을 제공하기 때문에 STL container 의 원소로 바로 사용해도 무리가 없다. 대신 멤버 변수들의 가독성 측면에서는 struct 가 좋다.

시간이 충분하다면 `std::pair` → `struct` 로 변경하는게 대부분의 경우에서 합당할 것이다.

구현해야하는 operator 가 그리 많은 것도 아니고, 대부분은 자명하게 구현되기 때문에 

그 구현 자체가 너무 verbose 하게 여겨 질 수도 있는데 그런 경우 STL 사용을 포기해야할 것