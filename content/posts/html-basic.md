---
title: "HTML Basic"
date: 2020-06-11T02:37:53+09:00
tags: ["programming", "web", "html"]
---

# INTRO
 공부하고 알아두면 좋을만한거 세상에 엄청 많다. 고등학교 교육과정에 뭔 과목이 그렇게 많았는지 생각해보면 사람마다 '뭘 알아두면 좋다' 라고 생각하는 기준이 다르기 때문인거 같다. 
 웹은 왜 공부해햘까? 고민해봤는데 생활코딩 강의 영상에서 너무 명쾌한 답변을 얻었다. 많이 쓰니까. 많이 쓰니까 배우면 좋은거지 나중에야 안쓰일 수도 있지만 지금 당장 많이 사용한다면 당장 공부를 시작할 동기는 충분하다. 

# CONETNS

## 기본 문법 - 태그

### 강조표시 

`<strong></strong>`


### 밑줄 

`<u></u>`


태그라는 문법이 있다는 걸 알면 이제 어떤 기능을 하는 태그냐만 알면 된다. 
모든 태그를 알필요는 없다.
필요해지면 찾아보고, 그때 사용해보면 된다. 
통계적으로 많이 사용하는 태그를 우선적으로 파악하자

![most-used-tags](https://s3-ap-northeast-2.amazonaws.com/opentutorials-user-file/module/3135/7624.png)


### 줄바꿈 

`<br>`  
html의 몇몇 태그는 이것처럼 태그를 닫지 않는다.

`<p></p>`  
단락 줄바꿈을 할때 사용하면 좋음
  

단락과 단락의 간격은 미리 정해져 있는 값이라 자유도가 떨어진다.  
CSS를 이용하면 세밀하게 조정이 가능하다.
  
```html
<p style="margin-top:45px;">
    HTML elements are delineated by tags, written using angle brackets.
</p>
```
  
contents들이 어떤 태그에 감싸져 있는지는 검색되는 결과에도 영향을 미친다.

`<stong>` 은 단순히 시각적인 강조 효과지만, `<h1>` 과 같은 태그는 그 자체로 contents 내부에서 해당하는 내용물이 어떤 중요도를 가지고 있는지 내포하고 있다.


### 이미지 

`<img>`

```html
<img src="https://s3-ap-northeast-2.amazonaws.com/opentutorials-user-file/module/3135/7648.png">
```


### 리스트

`<ul></ul>`

`<li></li>`


`<ul>`로 큰 구분, `<li>` 로 작은 구분을 할 수 있다. 



![img](https://s3-ap-northeast-2.amazonaws.com/opentutorials-user-file/module/3135/7664.png)



### 페이지의 제목

`<title></title>`

검색 엔진에서도 주요하게 쓰는 태그이기 때문에 사용하지 않으면 손해 !


### 링크

`<a></a>`

```html
<a href="https://www.w3.org/TR/html5/" target="_blank" title="html5 specification">
```



# REFERENCE

[WEB1 - HTML & Internet](https://opentutorials.org/course/3084)