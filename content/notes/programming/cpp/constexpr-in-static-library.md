---
title: "Constexpr in Static Library"
date: 2020-09-21
tags: ["cpp", "bug"]
---

정적 라이브러리 내부에서 constexpr 함수를 정의할 경우 외부에서 사용하는 것이 불가능하다. 

 가만 생각해보면 compile time에 가능한 연산을 미리 하는 것과, link time에 가능한 연산을 하는 것은 다른 개념이었다.

이런 점 때문에 많은 라이브러리들이 header-only 의 형태로 배포되는 것으로 보인다. 

