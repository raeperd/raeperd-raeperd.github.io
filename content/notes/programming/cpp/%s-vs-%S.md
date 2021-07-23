---
title: "%S vs %S"
date: 2020-11-21
tags: ["cpp", "bug"]
---

| | windows | linux |
|--|--|--|
|%s| wide character | multi-byte 문자열 |
|%S| multi-byte 문자열 | wide character |

- windows 와 linux 에서 반대로 동작한다.
- 양 플랫폼 모두에서 동작하게 하려면 두 문자열을 한번 wrapping 해서 사용해야한다.