---
title: "CSS Basic"
date: 2020-06-11T02:38:00+09:00
tags: ["programming", "web", "css"]
---



# INTRO

 HTML 이후에 글꼴도 바꾸고 싶고, 가운데 정렬도 하고싶고 등등 여러가지를 하고 싶은데 html 문법에는 한계가 있었다.   

새로운 태그를 추가할까? 새로운 언어를 개발할까? 

문서의 디자인은 정보라고 할 수없는데, html 내부에 디자인과 관련된 정보가 포함되버리면 너무 복잡해져 버린다.

기능과 UI는 분리될 필요가 있듯이, 디자인과 정보는 분리될 필요가 있다.

그래서 새로운 언어, CSS 가 필요했다.



# CONETNS



## CSS basic
``` html
<style>
  a {
    color: black;
  }
</style>
```
`<style></style>`

여기 태그의 내용은 css다. css의 문법에 맞게 처리해라. 

여러 `a` 태그 중에 특정 값만 스타일을 변경하고 싶다.
```html
<a href="y.html"> yy </a>
<a href="x.html", style="color:red"> xx </a>
```



## CSS structure

![image-20200524175346911](https://drive.google.com/uc?export=view&id=1K9By0UNFUenTRMPKzCf5oJDkt3sKobJG)


이제, 어떤 Property, 어떤 Value를 지정하는지만 알아내면 css로 코딩하는 것이 가능하다.



## CSS Selector

### tag selector (element selector)

```css
a {
    color: black;
}
```



### class selector


```css
.saw {
	color: red;
}
```

- `.` 으로 시작하는것이 class 의 속성이라는 것을 의미한다.

- class 는 여러개의 값을 가질 수 있다. (띄어쓰기만으로)

- 나중에 등장한 클래스의 값을 가지게 된다. 




### ID selector

```css
#active {
    color: blue;
}
```



tag 선택자 < class 선택자 < id 선택자

- id 는 한번만 등장해야한다.

- 포괄적인 선택자가 더 약해야 한다.
- 전체적인 디자인을 먼저 tag를 통해 하고, 예외를 class, id로 변경하는 디자인이 가능하다. 



[CSS Selector Reference](https://www.w3schools.com/cssref/css_selectors.asp)

<p class="codepen" data-height="265" data-theme-id="light" data-default-tab="html,result" data-user="reyzy" data-slug-hash="yLezPxr" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="css-selector">
  <span>See the Pen <a href="https://codepen.io/reyzy/pen/yLezPxr">
  css-selector</a> by ParkRaeCher (<a href="https://codepen.io/reyzy">@reyzy</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>



## Box model

h1은 줄바꿈을 하지만 a는 그렇지 않다. 어떤 태그는 이렇고 다른 태그는 이렇지 않다. 

직관적으로 제목은 전체 화면을 쓰는게 좋을 것 같고, 링크는 화면의 일부만 사용해도 좋을 것 같다.  



### Block level element

화면 전체를 쓰는 태그



### Inline element

라인 단위를 사용하는 태그 



 ![CSS | Box model - GeeksforGeeks](https://media.geeksforgeeks.org/wp-content/uploads/box-model-1.png)





<p class="codepen" data-height="265" data-theme-id="light" data-default-tab="html,result" data-user="reyzy" data-slug-hash="BajwJBO" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="box model">
  <span>See the Pen <a href="https://codepen.io/reyzy/pen/BajwJBO">
  box model</a> by ParkRaeCher (<a href="https://codepen.io/reyzy">@reyzy</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>



## Grid 

### div 

- 의미와 기능없이 디자인을 가지는 요소를 구분하기 위한 태그. 

- block level element



### span

- inline level element



### grid-template 

- `display: grid;`

- fr: frame 비율 





<p class="codepen" data-height="265" data-theme-id="light" data-default-tab="result" data-user="egoing" data-slug-hash="oGRaeM" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="oGRaeM">
  <span>See the Pen <a href="https://codepen.io/egoing/pen/oGRaeM">
  oGRaeM</a> by egoing (<a href="https://codepen.io/egoing">@egoing</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>



https://caniuse.com/



## Media Query

- 화면의 크기에 따라 웹의 표현이 달라졌으면 좋겠다.
- 반응형 웹 



<p class="codepen" data-height="265" data-theme-id="light" data-default-tab="html,result" data-user="reyzy" data-slug-hash="ExPwoLX" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Media Query">
  <span>See the Pen <a href="https://codepen.io/reyzy/pen/ExPwoLX">
  Media Query</a> by ParkRaeCher (<a href="https://codepen.io/reyzy">@reyzy</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>



## CSS  코드의 재사용

```html
<link rel="stylesheet" hreft="style.css">
```






# REFERENCE

[생활코딩 WEB2 CSS](https://opentutorials.org/course/3086)