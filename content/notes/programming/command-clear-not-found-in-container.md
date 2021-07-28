---
title: "Command Clear Not Found in Container"
date: 2020-11-02
tags: ["docker", "bug"]
---

간단하게 테스트 해보고 싶어서 centos8 컨테이너 하나를 올렸다.

```bash
 docker run --name pure-centos8 --rm -v D:\git:/git -ti centos:centos8 /bin/bash
```

```bash
 $ clear
 bash: clear: command not found
```

너무 황당해서 말도 안나오는 오류;;  

/usr/bin 경로에 clear가 있어야 할 것 같지만, 기본 컨테이너는 이마저도 가지고 있지 않다.

설치를 해줘야하는데, 아래 처럼 간단하게 해결할 수 있다.

```bash
 $ yum whatprovides /bin/clear
 Last metadata expiration check: 0:00:29 ago on Thu 11 Jun 2020 02:31:26 AM UTC.
 ncurses-6.1-7.20180224.el8.x86_64 : Ncurses support utilities
 Repo        : BaseOS
 Matched from:
 Filename    : /usr/bin/clear
```

물론 yum update 는 해줘야 잘 찾을 것  


저친구가 가지고 있다고 하니 설치만 하면 된다.
```bash
 $ yum install -y ncurses
```

man 도 안가지고 있다.
```bash
 $ yum install -y man
```

### **Thanks to**

[What Linux Package Install Clear, Install Clear On CentOS Linux](https://www.question-defense.com/2011/05/15/what-linux-package-install-clear-install-clear-on-centos-linux)