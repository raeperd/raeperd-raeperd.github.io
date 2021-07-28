# Reference vs Pointer

Category: cpp
Created: November 2, 2020 9:11 PM
Created By: RaeCheol Park
Last Edited Time: November 2, 2020 9:11 PM
Status: DONE

1. pointer는 NULL 값을 가질 수 있지만 reference는 NULL 값을 가질 수 없다.
    - 그래서 parameter가 pointer라면 NULL check를 해야하지만 reference 면 할 필요가 없다.
2. pointer는 할당할때 주소값을 할당하지만 reference는 대상을 그대로 할당한다. 
3. 선언 및 초기화
    - pointer는 선언만 하는 것이 가능하지만 레퍼런스는 선언과 동시에 초기화 하지 않으면 컴파일 오류가 발생한다.
4. 참조 대상 할당 및 접근
    - 포인터는 주소값을 할당하고 레퍼런스는 대상을 직접 할당한다.
5. 참조 대상 변경 가능 여부 
    - 레퍼런스는 A를 참조하다가 B를 참조하는 것과 같은 행위를 할 수 없다.

레퍼런스가 함수의 파라미터라면 대상을 그대로 전달 할 수 있다.