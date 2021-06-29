---
title: "About Vagrant"
date: 2019-10-25
tags: ["settings"]
draft: true
cover:
  image: /cover/vagrant.png
---

# INTRO

 최근에 제가 맡은 https://seculetter.atlassian.net/browse/MARS-476  의 경우 CentOS8 뿐만 아니라 CentOS6 (기존 환경) 에서도 slcli 의 동작을 확인할 필요가 있습니다. 기존 환경의 경우 설치스크립트를 이용해 초기화를 진행하는데 CentOS8 의 경우 아직 설치 스크립트가 없기 때문에 처음부터 설치 스크립트를 작성하면서 환경을 구성해야 합니다. 

 개발과 빌드를 하면서 OS의 설치 스크립트를 같이 관리하는 것은 여러므로 귀찮은 일이라 처음에는 Docker를 적용하려 했습니다. 하지만 이렇게 되면 로컬에서 Virtual Box 를 실행하지 못하기 떄문에 로컬에서 엔진 관련된 테스트를 진행할 수 없습니다. 

 이 과정에서 Vagrant 를 알게 되었고 현재는 Vagrant 를 활용해 개발 환경을 구성했습니다. 그 과정에 대해 공유 드리려 합니다.



# CONTENTS

## Vagrant

![6c2c517f-2fc6-4c65-b342-92774b7e361c](.images/6c2c517f-2fc6-4c65-b342-92774b7e361c.png)

 가상머신을 활용하기 위해서는 기본적으로 가상 머신 툴을 이용해 VM 을 생성하고 해당 VM에 OS 를 설치하는 반복적인 작업을 거쳐야 합니다. 설치 과정에서 생기는 여러 오류들을 구글링하고 수정하는 과정은 꽤 오랜 시간을 잡아먹게 되는데, Vagrant 를 활용하면 몇가지 간단한 명령어를 이용해 이를 쉽게 할 수 있습니다. 



 또 Vagrant 를 사용하면 여러 개발자간 일치하지 않는 작업 환경을 동일하게 구성할 수 있고 환경 구성으로 인한 귀찮은 작업에 쓰이는 시간들을 최소화 할 수 있습니다. 원격 빌드 머신과 유사한 환경을 Vagrantfile 로 공유하면 손쉽게 빌드 환경과 개발환경을 같게 할 수 있습니다. Docker를 아신다면, Docker가 Image 와 Container 를 관리하는 것처럼 가상 머신을 관리할 수 있게 한다고 생각하시면 됩니다. 



 vagrant 는 backend 로 virtaul box 또는 vmware 를 사용할 수 있습니다. 여기서는 virtual box 를 예로 들겠습니다. 



## Getting Started

1. Vagrant 설치 https://www.vagrantup.com/
2. Virtual Box 설치 https://www.virtualbox.org/wiki/Download_Old_Builds (5.2 설치)



지금부터는 CentOS8을 예로 들겠습니다.



3. 아래의 스크립트를 Vagrantfile 이라는 이름으로 저장합니다. 

```
Vagrant.configure("2") do |config|
  config.vm.box = "centos/8"
  config.vm.box_url = "http://cloud.centos.org/centos/8/x86_64/images/CentOS-8-Vagrant-8.1.1911-20200113.3.x86_64.vagrant-virtualbox.box"
end
```



4.  스크립트가 있는 경로에서 vagrant up 실행 

```
vagrant up
```

최초에 실행할때는 이미지를 받아오는 과정을 포함해서 시간이 좀 걸립니다. 



5. vagrant ssh 로 ssh 접속

```
vagrant ssh
```



6. exit 을 이용해 ssh  를 빠져나올 수 있으며, vagrant halt 로 가상머신을 종료합니다.

```
exit
vagrant halt
```



실행에 성공하면 아래처럼 virtual box 에서 이미지가 만들어진 것을 확인할 수 있습니다. 

virtual box 를 통해서 직접 가상머신에 접근할 수 있고 접근을 해보면 CentOS8 의 minimum 버전이 설치 된 것을 확인할 수 있습니다. 계정 정보는 기본적으로 ID: vagrant Password: vagrant 입니다. 



## 공유 폴더 설정

 vagrant 의 backend 가 virtual box 인 경우 공유 폴더를 사용하기 위해서는 Virtualbox GuestEddition 이 guest 머신에 설치되어 있어야합니다. 이 설치를 자동으로 해주는 [vagrant vbguet ](https://github.com/dotless-de/vagrant-vbguest)플러그인을 활용해 이를 쉽게 할 수 있습니다. 



1. Vagrant vbguest 설치

```
vagrant plugin install vagrant-vbguest
```



2. Vagrantfile 수정 

```
Vagrant.configure("2") do |config|
  config.vm.box = "centos/8"
  config.vm.box_url = "http://cloud.centos.org/centos/8/x86_64/images/CentOS-8-Vagrant-8.1.1911-20200113.3.x86_64.vagrant-virtualbox.box"
  
  config.vagrant.plugins = "vagrant-vbguest"
  config.vbguest.auto_update = false
  config.vbguest.no_remote = true

  config.vm.synced_folder "C:/git/", "/git"
end
```

 9번 라인으로 공유 폴더를 설정합니다. 앞의 경로가 host 뒤에 경로가 guest 에서 마운트 할 경로입니다. 



3. Vagrant up

```
vagrant up
vagrant vbguest 
vagrant relaod
```

위의 명령을 차례대로 입력합니다. vagrant reload 는 단순히 vagrant halt 와 vagrant up 을 실행합니다. 처음 실행할 경우 오래 걸릴 수도 있습니다. 자세한 내용은 [여기](https://github.com/dotless-de/vagrant-vbguest)를 참고하시면 됩니다. 



## 설치 스크립트 설정

1. install.sh 의 설치스크립트를 작성합니다. 

```
yum update -y

yum install -y gcc-c++
yum install -y cmake
yum install -y gdb
yum install -y readline-devel

yum install -y git

yum install -y openssh-server
```

현재까지 slcli가 빌드되기 위한 최소한의 설치 스크립트 입니다. gdb 는 빌드과정에서는 없어도 되지만 디버깅을 위해 설치합니다. 



2. Vagrantflie 을 아래와 같이 수정합니다.

```
Vagrant.configure("2") do |config|
  config.vm.box = "centos/8"
  config.vm.box_url = "http://cloud.centos.org/centos/8/x86_64/images/CentOS-8-Vagrant-8.1.1911-20200113.3.x86_64.vagrant-virtualbox.box"
  
  config.vagrant.plugins = "vagrant-vbguest"
  config.vbguest.auto_update = false
  config.vbguest.no_remote = true

  config.vm.synced_folder "C:/git/", "/git"

  config.vm.provision "shell", path:"install.sh"
end
```

11 번 라인으로 실행할 스크립트의 경로를 지정할 수 있습니다. 



3. vagrant provision 명령 실행

```
vagrant up
vagrant provision 
```



## ssh 설정 가져오기 

vagrant ssh-config를 이용하면  vagrant up 이 실행되어 있는 상황에서 ssh 로 접속할 수 있는 설정 파일을 쉽게 가져올 수 있습니다.

```
vagrant ssh-config
```



위 명령을 실행하면 아래와 같은 정보를 얻을 수 있습니다.

이를 활용해서 원격 개발을 할 수 있습니다. 



## 버전 관리 

 Dockerfile 과 마찬가지로 Vagrantfile 을 버전관리 하는 것 만으로 개발 환경과 빌드 환경에 대한 버전 관리를 할 수 있습니다. [이곳](https://bitbucket.seculetter.com/users/raecheol.park/repos/dev-env/browse)에 간단하게 저장소를 만들어 보았습니다. 

 Vagrantfile 과 Dockerfile 을 같은 곳에서 관리하고 실제 설치 스크립트인 install.sh를 공유하는 형식으로 docker container 와 vagrant box 를 같은 환경으로 유지할 수도 있을 듯 합니다. 

 지금 저장소에는 대응되는 Dockerfile 도 만들어서 docker 를 사용할 수 있는 환경에서는 docker로 slcli를 개발할 수도 있습니다. 이외에도 네트워크 설정이나 remote 개발을 위한 ssh 연결도 가능합니다. vscode 와 visual studio 모두 가능하지만 slcli 개발 환경에는 vscode 가 더 적합해 보이긴 합니다. 



## 기본 명령어 

```
vagrant up           # vagrant box 실행
vagrant halt         # vagarnt box 종료 

vagrant provision    # Vagrantfile 의 provision 실행 (설치 스크립트와 같은 것들을 실행)

vagrant ssh          # ssh 로 접속
vagarnt ssh-config   # ssh-conifg 정보 얻기

vagrant snapshot save SNAPSHOT_NAME       # snapshot 저장
vagrant snapshot resotre SNAPSHOT_NAME    # snapshot 으로 복귀
```

더 자세한 내용은 [여기](https://www.vagrantup.com/docs/cli/)를 참고하시면 됩니다. 



## 장단점 

장점 
- 간단하게 가상머신을 설치하고 실행할 수 있다.
- 버전관리를 쉽게 할 수 있다 
- 가상 머신의 설정을 파일로 관리 할 수 있다 

단점
- 최초에 실행하는데 오래 걸린다. 
- 설정 파일 문법. (Ruby 기반이고, 직관적이지 않다고 생각함) 

 Docker 와 유사한 인터페이스로 가상머신을 조작할 수 있는데 단점들은 모두 Docker 에 비한 단점이고 기술적으로 발생할 수 밖에 없는 불편함이라고 생각합니다. **다른 모든 부분에서 Docker가 더 좋다고 생각하지만, Docker 를 사용할 수 없는 환경에서 Vagrant는 사용하지 않을 이유가 없는 것 같습니다.**  윈도우 agent 도 Vagrant 로 관리할 수 있다면 더 좋을 것 같습니다. 

## 설치 오류 경험담

 저는 호스트 윈도우 10, vagrant 의 backend 로 virtualbox를 사용했습니다. 

**virtual box 5.1 버전에서 공유폴더 설정이 잘 안됬습니다.** 

→ 5.2 로 올리고서 깔끔하게 해결 

**centos6 를 ssh 원격 개발하기** 

 centos6 는 ssh 원격 개발을 지원하지 않습니다. vscode 로 개발하려면 guest 에 vscode-server 가 자동으로 설치되는데 centos6 에서는 이 과정이 잘 되지 않습니다. CentOS8 에서의 code intellisense 와 크게 다르지는 않아 빌드와 테스트만 따로 진행하면 됩니다. 

→ centos6 에서는 원격 개발을 하지 않고 빌드와 유닛테스트만 진행



# Reference 

[Vagrant Document](https://www.vagrantup.com/docs/index.html)

[Vagrant-vbguest](https://github.com/dotless-de/vagrant-vbguest)

[dev-env 저장소](https://bitbucket.seculetter.com/users/raecheol.park/repos/dev-env/browse)