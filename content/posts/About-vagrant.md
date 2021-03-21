---
title: "About vagrant"
date: 2020-03-06T22:57:03+09:00
tags: ["vagrant"]
---



# INTRO

 최근에 CentOS6 에서 동작하던 프로그램이 CentOS8 에서도 기능하도록 구현하는 이슈를 맡았다. 무작정 CentOS8 버전으로 업그레이드 하는 것이 아니라 호환성을 유지하면서 업그레이드를 해야하는 상황이었다. 기존 환경의 경우 설치스크립트를 이용해 초기화를 진행하는데 CentOS8 의 경우 아직 설치 스크립트가 없기 때문에 처음부터 설치 스크립트까지 같이 작성하면서 구현할 필요가 있었다. 

 개발과 빌드를 하면서 OS의 설치 스크립트를 같이 관리하는 것은 여러므로 귀찮은 일이라 처음에는 Docker를 적용하려 했다. 하지만 이렇게 되면 로컬에서 Virtual Box 를 실행하지 못하게 된다. 업무 특성상 Virtual box를 자주 사용하게 되는데 이럴 때마다 Hyperr-V를 껏다가 켯다가 하는 수고를 겪고 싶지는 않았다. 윈도우 10에서 Docker toolbox를 통해 억지로 할 수 있지만 성능 상 손해를 보기도 하고 Docker 에서도 권장하지 않는 방법이다. 

 다른 좋은 방법이 없을까 고민하다가 회사 동료 분이 Vagrant를 추천해셨다. Vagrant가 뭔지 듣자마자 내가 찾던 그거라는 것을 알게 됬는데 간단하게 특징과 사용법을 정리해보려고 한다. 

# CONTENTS

## About Vagrant

![image-20200306232801889](/images/vagrant-logo.png)

 가상머신을 활용하기 위해서는 기본적으로 가상 머신 툴을 이용해 VM(Virtual Machine)을 생성하고 해당 VM에 OS를 설치하는 반복적이고 지루한 작업을 거쳐야 합니다. 심지어 설치가 깨끗하게 진행되지도 않는다. 설치 과정에서 생기는 여러 오류들을 구글링하고 수정하는 과정은 꽤 오랜 시간을 잡아먹게 되는데, Vagrant 를 활용하면 몇가지 간단한 명령어를 이용해 이를 쉽게 할 수 있다. 

 또 Vagrant 를 사용하면 여러 개발자간 일치하지 않는 작업 환경을 동일하게 구성할 수 있고 환경 구성으로 인한 귀찮은 작업에 쓰이는 시간들을 최소화 할 수 있다. 원격 빌드 머신과 유사한 환경을 Vagrantfile 로 공유하면 손쉽게 빌드 환경과 개발환경을 같게 할 수 있습니다. Docker를 안다면, Docker가 Image 와 Container 를 관리하는 것처럼 가상 머신을 관리할 수 있게 한다고 생각하면 된다. 앞서 언급한 Vagrantfile 도 Dockerfile 과 같은 역할을 한다. 

 vagrant 는 backend 로 virtaul box 또는 vmware 등을 사용할 수 있는데, 여기서는 virtual box 를 예로 든다. 

## Getting Started

 실제로 내가 진행했던 CentOS8 을 설치하는 방법은 아래와 같다. 

1. Vagrant 설치 https://www.vagrantup.com
2. Virtual Box 설치 https://www.virtualbox.org/wiki/Download_Old_Builds (5.2 설치)
3. 아래의 스크립트를 Vagrantfile 이라는 이름으로 저장
4. Vagrantfile이 있는 경로에서 vagrant up 실행 
5. 같은 경로에서 vagrant ssh 로 ssh 접속
6. exit 을 이용해 ssh  를 빠져나올 수 있으며, vagrant halt 로 가상머신을 종료  

```
Vagrant.configure("2") do |config|
  config.vm.box = "centos/8"
  config.vm.box_url = "http://cloud.centos.org/centos/8/x86_64/images/CentOS-8-Vagrant-8.1.1911-20200113.3.x86_64.vagrant-virtualbox.box"
end
```

```shell
$ vagrant up
```

```shell
$ vagrant ssh
```

최초에 실행할때는 이미지를 받아오는 과정을 포함해서 시간이 좀 걸린다. 

```
$ exit
$ vagrant halt
```

실행에 성공하면 아래처럼 virtual box 에서 이미지가 만들어진 것을 확인할 수 있다. 

![vagrant-on-virtual-box-ui](/images/vagrant-on-virtual-box-ui.png)

virtual box 를 통해서 직접 가상머신에 접근할 수 있고 접근을 해보면 CentOS8 의 minimum 버전이 설치 된 것을 확인할 수 있다. 계정 정보는 기본적으로 ID와 password는 vagrant 이다. 

![image-20200306233002675](/images/vagrant-on-virtual-box-inside.png)

 

## 공유 폴더 설정

 Virtual box 로 가상머신을 사용하면서 또 많이 사용하는게 공유폴더 기능이다. 이것도 조금 해맬 수 있고 오류가 많이 나는 부분인데 Vagrant에서는 이 과정도 script 로써 간단한 몇가지 명령으로 가능하다. 
 Vagrant 의 backend 가 virtual box 인 경우 공유 폴더를 사용하기 위해서는 Virtualbox GuestEddition 이 guest 머신에 설치되어 있어야합니다. (Virtual box만 사용할때도 필요하다) 이 설치를 자동으로 해주는 [vagrant vbguet](https://github.com/dotless-de/vagrant-vbguest) 플러그인을 설치함으로써 이를 쉽게 할 수 있다.


1. Vagrant vbguest 설치

```shell
$ vagrant plugin install vagrant-vbguest
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

 9번 라인으로 공유 폴더를 설정한다. 앞의 경로가 host 뒤에 경로가 guest 에서 마운트 할 경로다. 


3. Vagrant up

```
$ vagrant up 
$ vagrant vbguest  
$ vagrant relaod
```

위의 명령을 차례대로 입력한다. 
`vagrant reload` 는 단순히 `vagrant halt` 와 `vagrant up` 을 실행합니다. 처음 실행할 경우 오래 걸릴 수도 있고, `vagrant vbguest` 는 최초에만 실행하고 이후에는 안해도 된다. 가끔 공유 폴더가 `vagrant up` 만으로 설정되지 않는 경우가 있는데 그럴 때만 다시 `vagrant vbguest` 를 해주면 된다. 더 자세한 내용은 [여기](https://github.com/dotless-de/vagrant-vbguest)를 참고하시면 된다. 

 

## 설치 스크립트 설정

1. install.sh 의 설치스크립트를 작성한다. 

```shell
yum update -y

yum install -y gcc-c++
yum install -y cmake
yum install -y gdb
yum install -y readline-devel

yum install -y git

yum install -y openssh-server
```


2. Vagrantflie 을 아래와 같이 수정한다.

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

`config.vm.provision "shell", path:"install.sh"` 로 실행할 스크립트의 경로를 지정할 수 있다. 


3. vagrant provision 명령 실행

```shell
$ vagrant up
$ vagrant provision 
```

마찬가지로 `vagrant provision` 명령은 최초에 한번, 그리고 설치 스크립트가 수정될 때만 실행해주면 된다. 


## ssh 설정 가져오기 

vagrant ssh-config를 이용하면  vagrant up 이 실행되어 있는 상황에서 ssh 로 접속할 수 있는 설정 파일을 바로 가져올 수 있다.

```shell
$ vagrant ssh-config
```

그대로 복사해서 vscode 의 ssh 설정 파일로 사용하면 바로 접속해서 원격 개발을 시작할 수 있다. (가장 편했던 부분!)


## 버전 관리 

 Dockerfile 과 마찬가지로 Vagrantfile 을 버전관리 하는 것 만으로 개발 환경과 빌드 환경에 대한 버전 관리를 할 수 있다. 

 또 Vagrantfile 과 Dockerfile 을 같은 곳에서 관리하고 실제 설치 스크립트인 install.sh를 공유하는 형식으로 docker container 와 vagrant box 를 같은 환경으로 유지할 수 있지 않을까 싶었는데 두 Dockerfile 에서 설치스크립트를 상위 폴더로의 상대 접근으로 접근 할 수 없어서 불가능 했다. 두 환경을 같게 유지하려면 Dockerfile <-> Vagrantfile 의 변환을 스크립트로 작성하면 될 듯 하다.

스크립트 작성까지는 아직 오버하는 것 같아서 Dockerfile과 Vagrantfile을 모두 한곳에서 관리 하고 있는데, 집에서는 Dockerfile로 개발하고 회사에서는 Vagrantfile 로 개발하는식으로 적용하고 있다. 


## 기본 명령어 

위에서도 몇가지 명령어를 직접 언급했지만, 간단하게 자주 사용하는 명령어는 아래와 같다. 

```shell
$ vagrant up           # vagrant box 실행
$ vagrant halt         # vagarnt box 종료 

$ vagrant provision    # Vagrantfile 의 provision 실행 (설치 스크립트와 같은 것들을 실행)

$ vagrant ssh          # ssh 로 접속
$ vagarnt ssh-config   # ssh-conifg 정보 얻기

$ vagrant snapshot save SNAPSHOT_NAME       # snapshot 저장
$ vagrant snapshot resotre SNAPSHOT_NAME    # snapshot 으로 복귀

```

더 자세한 내용은 [여기](https://www.vagrantup.com/docs/cli/)를 참고하면 된다.

 

## 장단점 

장점 

- 간단하게 가상머신을 설치하고 실행할 수 있다.
- 버전관리를 쉽게 할 수 있다 
- 가상 머신의 설정을 파일로 관리 할 수 있다 


단점

- 최초에 실행하는데 오래 걸린다. (Docker와 비고해서)
- 설정 파일 문법. (Ruby 기반이고, 직관적이지 않다고 생각함) 


 Docker 와 유사한 인터페이스로 가상머신을 조작할 수 있는데 단점들은 모두 Docker 에 비한 단점이고 기술적으로 발생할 수 밖에 없는 불편함이라고 생각한다. **다른 모든 부분에서 Docker가 더 좋다고 생각하지만, Docker를 사용할 수 없는 환경에서 Vagrant는 사용하지 않을 이유가 없는 것 같습니다.**  윈도우 Virtual box 도 Vagrant 로 관리할 수 있는 것 같은데, 이 것도 적용해보면 좋을 것 같다.

 

## 설치 오류 경험담 (삽질 경험담)

호스트 윈도우 10, Backend 로 virtualbox를 사용. 


### virtual box 5.1 버전에서 공유폴더 설정

→ 5.2 로 올리고서 깔끔하게 해결 

구 버전, 공식적으로 지원하지 않는 버전을 사용하지 않는게 얼마나 중요한건지 다시 알게됨. 간단한 문제였는데 다른 프로그램들 때문에 섣불리 버전을 못올리고 있었다. 어떻게든 버전을 안올리고 해결해보려 했는데 잘 안됬다. 꽤나 오랫동안 삽질함 .. 2~3시간정도?


### visual studio로 ssh 원격 개발 설정

 프로젝트가 out of source 형태로 빌드 되서 의존성을 처리하는데 애를 많이 먹었다. 절대 경로를 맞춰서 소스를 복사하게 만들어서 빌드가 되게 할 수 있겠지만 성능도 잘 안나오는 것 같고, code intellisense가 제대로 동작하지 않아 기껏 성공했지만 짜증만 났다.

→ vscode 에서 개발하는 것으로 해결 

이번에 알게 된건데 Visual studio와 Visual studio code의 원격 개발 지원 방식이 좀 다른것 같다. 전자는 파일을 복사해서 빌드하는 느낌이고 후자는 말그대로 가상 환경에 들어가서 개발하는 느낌이었는데, 후자를 원했지만 Visual studio 에서는 이를 지원하는 것 같아 보이지는 않는다. [Visual studio cloud](https://azure.microsoft.com/ko-kr/products/visual-studio/) 라는게 있지만 이건 또 다른 얘기 같다. 


### centos6 를 ssh 원격 개발하기

 이건 꽤 단순하다 centos6 는 ssh 원격 개발을 지원하지 않는다. vscode 로 개발하려면 guest 에 vscode-server 가 자동으로 설치되는데 centos6 에서는 이 과정이 지원 되지 않는다. CentOS8 에서의 code intellisense 와 크게 다르지는 않아 빌드와 테스트만 따로 진행하면 돤다. 귀찮긴 하지만 

→ centos6 에서는 원격 개발을 하지 않고 빌드와 유닛테스트만 진행

# Reference 

[Vagrant Document](https://www.vagrantup.com/docs/index.html)

[Vagrant-vbguest](https://github.com/dotless-de/vagrant-vbguest)

[dev-env 저장소](https://bitbucket.seculetter.com/users/raecheol.park/repos/dev-env/browse)