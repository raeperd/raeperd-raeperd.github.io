# Flexible date between query

Category: java
Created: January 29, 2021 6:56 PM
Created By: RaeCheol Park
Last Edited Time: January 29, 2021 6:56 PM
Status: NEED_DETAIL

두 날짜 사이에 생성된 사용자 정보를 모두 가져오려고 한다. 

처음엔 두 날짜의 사이로 구현을 했었다. 예를 들면, 2020-01-29 < date && date < 2020-01-30

양쪽 모두 equal 을 넣지 않고 비교하는 방법을 선택했었는데 이러면 여러가지 문제점이 생긴다.

대표적인 불편함은 예를 들어 2020-01-29 하루 동안의 결과를 얻고 싶다면, 

2020-01-28 < date && date < 2020-01-30 와 같은 어색한 요청을 해야하는데 원하는 날짜보다 하루 이전인 2020-01-28을 써야함이 불편하게 느껴진다.

그렇다고 양쪽에 모두 equal 을 넣은 API 를 오픈한다면 

2020-01-29 ≤  date && date ≤ 2020-01-29 와 같이 같은 날짜를 두번 입력하게 만드는데, 이거도 뭔가 어색하다. 

한쪽을 열어두는 쿼리를 구현하면 되는데, 사실 이 논리는 이전부터 많이들 고민했던 주제더라. 이 문제에서의 정답은 프로그래밍 언어에서 왜 인덱스가 0 부터 시작하는지와 일맥상통한다. 

[E.W. Dijkstra Archive: Why numbering should start at zero (EWD 831)](https://www.cs.utexas.edu/users/EWD/transcriptions/EWD08xx/EWD831.html)