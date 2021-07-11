---
title: Euclidean Algorhtim
date: 2017-07-25
tags: ["algorithm"]
---

# Base

![./image/Untitled.png](https://i.ibb.co/ZXj48s1/Untitled.png)

# Algorithm

```cpp
int euclidean(int a, int b) {
	int r1 = a;
	int r2 = b;
	int q,r;
	
	while (r2 > 0) {
		q = r1/r2;
		r = r1%r2;
		r1 = r2;
		r2 = r;
	}
	
	return r1;
}
```

# Algorithm (Recursive)

```cpp
int euclidean(int a, int b) {
	if (b==0)
        return a;
    else 
        return euclidean(b,a%b);
}
```

# Example

![./image/Untitled1.png](https://i.ibb.co/9vbzrLZ/Untitled1.png)

# 조금 더 직관적인 계산 방법

![./image/Untitled2.png](https://i.ibb.co/ZVW2pk2/Untitled2.png)

# 추가 예제)

![./image/Untitled3.png](https://i.ibb.co/qp5kHmD/Untitled3.png)

- 손으로 해보면 금방 감옴!

## Complexity of Euclidean Algorithm

![./image/Untitled4.png](https://i.ibb.co/3pzfmDj/Untitled4.png)

# Extended Euclidean Algorithm 원리

![./image/Untitled5.png](https://i.ibb.co/y4BhqKV/Untitled5.png)

- 목적은 위를 만족하는 s와 t를 찾는 것이다.
- Euclidean Algorihm을 거꾸로 타고 올라가면 된다.

## 방법이 자주 해깔렸는데,

![./image/Untitled6.png](https://i.ibb.co/hVSxsxm/Untitled6.png)

- 알고리즘을 진행하면서 생기는 나머지들에 대해 정리하면서, Euclidean Algorithm을 거꾸로 적용시킨다.

## Extended Euclidean Algorithm Process

![./image/Untitled7.png](https://i.ibb.co/hsnGts7/Untitled7.png)

## Extended Euclidean Algorithm Algorithm

```cpp
void extended_euclidean(int a, int b) {
    int s,s1,s2;
    int t,t1,t2;
    int q,r;
    int x,y;

    // initialize 
    x=a;
    y=b;
    if (a>b) { 
        s1 = 1;
        s2 = 0;
        t1 = 0;
        t2 = 1;
    }
    else {              
        s1 = 0;
        s2 = 1;
        t1 = 1;
        t2 = 0;
    }

    // run algorithm
    while (b>0) {
        q = a/b;
        r = a%b;
        a = b;
        b = r;
        
        s = s1;
        s1 = s2;
        s2 = s2 - s*q;

        t = t1;
        t1 = t2;
        t2 = t2 - t*q;
    }

    // test result
    if (a == s1*x+t1*y)
        printf("TRUE\n");
    else {
        printf("GCD : %d \n", b);
        printf("s : %d \n", s1);
        printf("t : %d \n", t1);
        printf("FALSE\n");
    }
}

int main(void) {
    extended_euclidean(10,2000);
    extended_euclidean(2000,10);
    return 0;
}
```