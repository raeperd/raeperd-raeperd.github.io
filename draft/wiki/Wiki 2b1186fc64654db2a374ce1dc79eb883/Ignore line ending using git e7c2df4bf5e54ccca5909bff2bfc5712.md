# Ignore line ending using git

Category: git
Created: November 2, 2020 9:08 PM
Created By: RaeCheol Park
Last Edited Time: November 2, 2020 9:08 PM
Status: DONE

윈도우에서 코딩하다가 리눅스로 넘어왔을때, line ending 이 바뀌었다는 이유로 git이 파일의 바뀌었다고 알릴때가 있다.
대부분의 경우 이런 변화는 버전관리에 포함시킬 필요가 없다.

커맨드 하나로 해결된다.

```bash
 $ git config --global core.autocrlf true
```

도커 컨테이너로 개발환경을 만들거나 할때 Dockerfile에 분명 git 을 설치하는 과정이 포함될텐데, 이 커맨드도 같이 넣어주는게 좋을 것 같다.
매번 잊고 있다가 버전관리에 들어오면 추가하게 되네