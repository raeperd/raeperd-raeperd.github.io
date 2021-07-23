---
title: "Error C2039 Memchar Is Not a Member of Global Namespace"
date: 2019-08-28
tags: ["cpp", "bug"]
---

내가 작성하고 있는 프로그램에서 string.h 라는 파일을 사용하고 있었다.

standard 에서 사용하는 이름은 내가 사용하면 안된다. 이런 보편적인 이름은 애초에 피하는게 좋다.

[error C2039: 'memchr' : is not a member of '`global namespace''](https://stackoverflow.com/questions/531916/error-c2039-memchr-is-not-a-member-of-global-namespace/531928)
