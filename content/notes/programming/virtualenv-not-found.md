---
title: "Virtualenv Not Found"
date: 2019-12-21
tags: ["python", "bug"]
---

# 현상
![virtualenv%20command%20not%20found%2029dd896c26ff4a99bed12b8e319f3d0a/Untitled.png](https://i.ibb.co/71LzqNr/Untitled.png)

단순히 사용자 권한으로 virtualenv 를 실행할때는 가능하고 root 권한으로 실행하니까 찾을 수 없단다.
pip install virtualenv 와 같은 형식으로 설치할 경우 이런 에러를 발견했다.

# 해결방안
[Virtualenv Command Not Found](https://stackoverflow.com/questions/31133050/virtualenv-command-not-found)

virtualenv 는 기본적으로 /user/local/bin 에 위치하는데 이 경로가 $PATH 에 존재하지 않아서 sudo 로는 못찾는 거다 user 권한으로 실행하는경우에는 이게 $PATH에 있는것처럼 동작하는 것 같다.

여튼 해결방법은 /usr/lcoal/bin 경로를 PATH 에 추가하는 것이다.

export PATH=$PATH:새로 등록할 프로그램의 주소

**그냥 yum install -y python3-virtualenv 가 좀더 깔끔한 해결책이었다.**

pip install 로 설치하면 귀찮은 경로 문제를 겪어야 한다. 다른 파이썬 라이브러리들이 global interpreter 를 오염시키는건 짜증나는 일이지만 매일 같이 사용할 virtualenv 는 global 하게 설치하는게 올바른 것 같다. 사실 virtualenv 는 파이썬 정식 라이브러리가 되어야 하지 않을까??
