---
title: "Gradlew Permission Denied"
date: 2021-01-21
tags: ["java", "bug"]
---

bamboo 에서 빌드 자동화 스크립트에서 단순히 `./gradlew clean build` 를 실행하게 했더니 ./gradlew 의 실행권한이 없단다. 구체적은 오류 메시지는 아래와 같다.

```
$ ./gradlew clean build
-bash: ./gradlew: Permission denied
```

bamboo agent가 git clone 을 할때 어떤 user의 어떤 permission으로 실행하는지, 혹은 directory 의 permission 등등 여러 문제가 겹쳐서 이런 문제가 생길 수 있다고 생각했다.

bamboo agent는 권한 문제가 없는 것을 확인하고 나서는 단순히 빌드를 실행하기 전에 chmod로 실행 가능하게 권한을 주는 방법으로 수정했었고 잘 동작하는 것을 확인했다.

근데 본질적인 해결방법은 이게 아니라  이거였다.

[gradlew: Permission Denied](https://stackoverflow.com/a/49072563/13662830)

windows 에서 git init 혹은 spring initializer 를 사용하면 chmod 의 default가 +x 가 아니였었다.

git의 커멘드로 관리할 수 있었고, bamboo build script에서 쓸때 없는 명령 한줄을 지울 수 있었다. 

이렇게 처리해야 한다

```bash
git update-index --chmod=+x gradlew
```