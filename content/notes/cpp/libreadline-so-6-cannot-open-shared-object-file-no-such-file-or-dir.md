---
title: "Libreadline So 6 Cannot Open Shared Object File No Such File or Dir"
date: 2019-10-21
tags: ["cpp", "bug"]
---

기본적인 해결방법은 

```bash
$ yum install -y readline-devel
```

설치 되고 나서도 여전히 같은 오류를 확인했다. 라이브러리를 찾아보면

```bash
$ find / -name 'libreadline.*'
```

```
/usr/lib64/libreadline.so.7
/usr/lib64/libreadline.so.7.0
/usr/lib64/libreadline.so
```

이런식으로 결과를 찾을 수 있는데 설치되어 있는건 7 버전이지만 바이너리에서 6버전을 찾고 있다. 빌드된 환경과 배포된 환경에서 컴파일러 버전이 다르면 이런 문제가 생길 수 있다.

```bash
ln -s /usr/lib64/libreadline.so.7 /usr/lib64/libreadline.so.6
```

이런식으로 링크를 만들어주면 간단하게 해결 할 수 있다.

이후에 libhistroy.so.6 을 찾으면서 같은 오류 메세지를 만드는데, 같은 방법으로 해결할 수 있다.

