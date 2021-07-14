---
title: "Why Return Size T"
date: 2019-11-21
tags: ["cpp"]
---

`std::string.size()` 와 같은 함수를 사용하면 반환되는 값은 `size_t` 형태이다. `unsinged int` 와 같은 자료형이여도 되는데 왜 하필이면 `size_t` 인걸까?

사실 `size_t` 는 `using size_t = unsinged int` 정도의 자료형이라고 생각했었는데 아니다. `size_t`는 특정 시스템에서 이론적으로 저장할수 있는 가장 큰 사이즈를 저장할 수 있다.

사이즈를 저장할 수 있다는 말이 해깔리는데, 그냥 32bit 환경에서는 32bit 자료형이고, 64bit 자료형에서는 64bit라는 소리다. ([cppreference - size_t](https://en.cppreference.com/w/cpp/types/size_t))

`unsinged int`와 같다는 건 32비트 환경에서만 그렇다. `size_t`는 64비트 환경에서는 `unsinged long long`과 같다.

