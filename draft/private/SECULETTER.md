# QA 

## 수정 사항

## 배포 환경

- QA 버전으로 배포 완료 
  - 서비스: [https://threat-intelligence-release.azurewebsites.net](https://threat-intelligence-release.azurewebsites.net/)
  - 로그서버: http://20.194.18.41:5601/app/discover#/?_g=(filters:!(),refreshInterval:(pause:!t,value:0),time:(from:now-15m,to:now))&_a=(columns:!(_source),filters:!(),index:'3dfa6c50-dd31-11eb-b1a1-2388aa3eb52c',interval:auto,query:(language:kuery,query:''),sort:!())

**QA 환경의 로그서버 주소가 변경됬습니다. 확인바랍니다**



## QA 확인 사항











## 수정 사항

- URL 진단시 URLhaus API 를 활용한 URL 조회 기능 구현 
- URL 진단 로직 수정 

1. 도메인 주소를 기준으로 조회 
2. URLhaus 조회 
3. Cyren 조회 
4. 결과 반환 

조회 결과가 발견 된 경우, 이후 과정을 거치지 않고 즉시 결과를 반환함 

## 배포 환경

**QA 환경의 로그서버 주소가 변경됬습니다. 확인바랍니다**

QA 버전으로 배포 완료

- 서비스: [https://threat-intelligence-release.azurewebsites.net](https://threat-intelligence-release.azurewebsites.net/)
- 로그서버: [http://20.194.18.41:5601/app/discover#/?_g=(filters:!(),refreshInterval:(pause:!t,value:0),time:(from:now-15m,to:now))&_a=(columns:!(_source),filters:!(),index](http://20.194.18.41:5601/app/discover#/?_g=(filters:!(),refreshInterval:(pause:!t,value:0),time:(from:now-15m,to:now)):'3dfa6c50-dd31-11eb-b1a1-2388aa3eb52c',interval:auto,query:(language:kuery,query:''),sort:!())
  - 기존의 azure 실배포 환경과 동일한 로그 서버를 사용하도록 변경되었습니다. 
  - index를 logstash → threat-intelligence-release 로 변경해주시면 됩니다. 

## QA 확인 사항

- URLhaus 서비스로 정상적인 조회가 이루어지는 지 확인
- URLhaus 연동이 된 버전(QA 버전)과 그렇지 않은 버전(azure master)간의 성능 차이 확인