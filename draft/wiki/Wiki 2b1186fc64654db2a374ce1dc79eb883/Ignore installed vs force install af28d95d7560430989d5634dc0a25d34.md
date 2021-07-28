# Ignore installed vs force install

Category: python
Created: November 2, 2020 9:06 PM
Created By: RaeCheol Park
Last Edited Time: November 2, 2020 9:06 PM
Status: DONE

파이썬 패키지를 배포할때 requirements.txt 나 setup.py를 이용해 많이 배포를 한다. 최근에 requirements에 있는 라이브러리 중 일부가 이미 시스템에 설치 되어있을때 문제가 생기는 경우를 확인 했다.

pyyaml 라이브러리를 설치하는데 기존 환경에서 이거보다 낮은 버전의 라이브러리가 설치되어 있는 경우 설치하지 않고 넘어갔다가, 런타임에 버전차이로 인한 오류를 확인할 수 있었다.

환경에 따라 혹은 pip 버전에 따라 이미 설치된 라이브러리가 있을 경우, pip가 어떻게 행동할지 예상하기 어렵다.

이를 명시적으로 설치할때 옵션으로 줄 수 있다.

### **`--force-reinstall`**

설치하기 전에 라이브러리가 존재한다면, 삭제하고 다시 설치하도록 한다.

### **`--ignore-installed`**

이미 설치된 라이브러리가 존재한다면, 거기에 파일을 덮어쓴다.
가령 버전이 달라짐에 따라 추가된 .py 파일이 있다면 그만큼 추가 될 것이다. .py 파일의 이름이 변경되면 예상하지 못한 문제를 만들 수도 있다.

이런 옵션을 주지 않았을떄 어떻게 동작하는지는 물론 문서를 보면 나오지만, 그냥 명시적으로 옵션을 주면 그런거 확인할 필요 없이 확인할 수 있다.

결론적으로, 보통의 경우에는 `--force-reinstall` 옵션을 사용하는 것이 옳아 보인다.

pip 뿐만 아니라 이런 의존성을 관리해주는 패키지를 사용할때면, 가려지는 default option들은 의도적으로 표현해 주는게 좋아보인다.

### **Thanks to**

[Difference between pip install options “ignore-installed” and “force-reinstall”](https://stackoverflow.com/questions/51913361/difference-between-pip-install-options-ignore-installed-and-force-reinstall)