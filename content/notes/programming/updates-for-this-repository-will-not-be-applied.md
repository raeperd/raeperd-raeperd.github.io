---
title: "Updates for This Repository Will Not Be Applied"
date: 2020-07-21
tags: ["docker", "bug"]
---

apt-get update 를 하는데 특정 주소에서 아래와 같은 에러가 뜨기 시작했다.

```bash
Hit:1 ubuntu bionic InRelease
Ign:3 linux/chrome/deb stable InRelease                   
Get:2 /ubuntu bionic-updates InRelease [88.7 kB]   
Get:5 /linux/chrome/deb stable Release [943 B]             
Get:6 http://dl.google.com/linux/chrome/deb stable Release.gpg [819 B]         
Get:4 http://us.archive.ubuntu.com/ubuntu bionic-backports InRelease [74.6 kB] 
Get:7 http://security.ubuntu.com/ubuntu bionic-security InRelease [83.2 kB]    
Reading package lists... Done                                 
E: Release file for http://dl.google.com/linux/chrome/deb/dists/stable/Release is not valid yet (invalid for another 2h 45min 28s). Updates for this repository will not be applied.
E: Release file for http://us.archive.ubuntu.com/ubuntu/dists/bionic-updates/InRelease is not valid yet (invalid for another 4h 34min 33s). Updates for this repository will not be applied.
E: Release file for http://us.archive.ubuntu.com/ubuntu/dists/bionic-backports/InRelease is not valid yet (invalid for another 1h 22min 16s). Updates for this repository will not be applied.
E: Release file for http://security.ubuntu.com/ubuntu/dists/bionic-security/InRelease is not valid yet (invalid for another 4h 32min 36s).
```

구글링으로 다른 사람들도 이런 문제를 겪었다는 걸 봤는데 sudo 명령을 쓰는걸 보니 컨테이너 환경은 아닌 것 같다. 

나는 기본적으로 리눅스 컨테이너에서 이런 오류가 났다. 

한시간정도 삽질을 하다가

미묘하게 (invalid for another 2h 45min 28s) 날짜가 container 의 date 와 현재 내 로컬에서의 시간이라는 것을 알게 됬다. 

그리고 또 그정도 기간동안 내가 docker 를 끄지 않았다는 것 까지 떠오르면서 컴퓨터를 재부팅해봤다.

그러니 되더라 .. 

신기하게 추리해서 해결했다.