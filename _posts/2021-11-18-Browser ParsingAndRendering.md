---
layout: single
title: "브라우저 파싱과 렌더링"
---
# 브라우저 파싱과 렌더링

서버에서 응답받은 HTML 문서를 브라우저에서 시각적으로 출력하기 위해서는 렌더링 과정이 필요하다. 

큰 흐름은 다음과 같다.
```
  브라우저의 렌더링 엔진은 텍스트 파일 즉 HTML 문서를 파싱하여 DOM을 생성하는데
  중간에 link 태그, style 태그를 만나면 HTML 파싱을 중단하고 CSS파싱하여 CSSOM을 생성한다. 
  그리고 생성된 DOM과 CSSOM은 렌더링을 위해 렌더 트리로 결합된다. 렌더 트리는
  HTML 요소의 레이아웃을 계산하는데 사용되며 브라우저 화면에 픽셀을 렌더링 하는 페인팅 처리에 입력된다.
```
  **렌더링**은 HTML 파일을 파싱하여 브라우저에 시각적으로 출력하는 과정  
  **파싱**은 트리 구조의 자료구조인 파스트리를 생성하는 일련의 과정  

***

## HTML 파싱

1) 브라우저는 서버에 보낸 요청을 **2진수** 형태로 응답받음
2) meta태그의 charset 어트리뷰트에 지정된 인코딩 방식을 기준으로 **문자열** 변환  
3) 문법적 의미를 갖는 코드의 최소단위인 **토큰**으로 분해  
4) 토큰들은 객체로 변환하여 **노드**(DOM을 구성하는 기본 요소)들을 생성  
5) 노드들로 구성된 트리 자료구조인 **DOM** 생성

#### 1. 브라우저는 서버에 보낸 요청을 **2진수** 형태로 응답받음
```
  ex) 1001011001011...
```

#### 2. meta태그의 charset 어트리뷰트에 지정된 인코딩 방식을 기준으로 문자열 변환    

 ```
  <html><head><meta charset="UTF-8">...</html>
 ```

#### 3.  문법적 의미를 갖는 코드의 최소단위인 토큰으로 분해

  ```
  {  
    startTag: 'html',
      contents: {
        startTag: 'head',
        contents: { ... },
        ...
      endTag:'html'
  }
  ```

#### 4. 토큰들은 객체로 변환하여 노드(DOM을 구성하는 기본 요소)들을 생성

```
  html  head  meata  link  body ...
```

노드는 4가지 종류가 있다.
문서 노드, 요소 노드, 어트리뷰트 노드, 텍스트 노드
  * 문서 노드(Document Node)
    * 트리의 최상위에 존재. 각각 요소, 어트리뷰트, 텍스트 노드에 접근하려면 문서 노드를 통해야 한다. 즉, DOM tree에 접근하기 위한 시작점(entry point)이다.
  * 요소 노드(Element Node)
    * 요소 노드는 HTML 요소를 표현. 어트리뷰트, 텍스트 노드에 접근하려면 먼저 요소 노드를 찾아 접근해야 한다.
  * 어트리뷰트 노드(Attribute Node)
    * 어트리뷰트 노드는 해당 어트리뷰트가 지정된 요소의 자식이 아니라 해당 요소의 일부로 표현된다. 따라서 해당 요소 노드를 찾아 접근하면 어트리뷰트를 참조, 수정할 수 있다.
  * 텍스트 노드(Text Node)
    * 텍스트 노드는 HTML 요소의 텍스트를 표현

#### 5. 노드들로 구성된 트리 자료구조인 DOM 생성

HTML문서는 HTML요소들의 집합이며 HTML요소는 중첩 관계를 갖는다. 

``` javascript
  <body>
    <div> //div 요소
      <div></div> //div 요소 내에 중첩된 div요소
    </div>
  </body>
```

중첩관계에 의해 부자관계가 형성되고, HTML 요소 간의 부자 관계를 반영하여 모든 노드들을 트리 자료구조를 DOM이라 한다. DOM은 메모리에 적재되어 렌더링시 사용된다.

<img src="https://user-images.githubusercontent.com/87258182/142354625-0dba96fb-e463-43f8-826e-ec729712aa56.png" width="70%" height="70%">

##### 결론적으로 HTML문서의 파싱의 결과는 DOM이다.


## CSS 파싱

1. 렌더링 엔진이 HTML을 파싱하다가 link 태그 또는 sytle태그를 만나면 DOM 생성을 일시 중단한다.
2. 어트리뷰트에 지정된 css파일을 서버에 요청
3. HTML파싱과 동일한 파싱과정을 거쳐 CSSOM 생성  
  (2진수 -> 문자 -> 토큰 -> 노드 -> CSSOM )
4. HTML 파싱이 중단된 지점부터 다시 HTML 파싱

CSSOM은 CSS의 상속을 반영
ex) body요소에 font-size 지정시 하위 요소들에도 상속을 반영하여 적용

<img src="https://user-images.githubusercontent.com/87258182/142354902-a83e1f5f-ee45-43a3-b5a5-0d1688fda8c2.png" width="70%" height="70%">

CSS의 파싱의 결과는 CSSOM이다.

## 렌더 트리 생성

DOM과 CSSOM은 렌더링을 위해 렌더 트리로 결합된다. 렌더 트리에는 meta 태그, script 태그 등 실제 화면에 표현되지 않는 노드들은 제거되고 화면에 표현되는 노드들로만 구성된다.

<img src="https://user-images.githubusercontent.com/87258182/142354979-f6ac2c16-bda8-4e13-9b77-9737c8443d58.png" width="80%" height="80%">

## Layout

렌더 트리 노드들이 갖고 있는 스타일과 속성에 따라서 브라우저 화면의 위치와 크기를 계산한다. 즉 레이아웃을 계산한다.

## Paint

브라우저 화면에 픽셀을 렌더링한다.

***

## Javascript 파싱

1. HTML문서를 파싱하다 script태그를 만나면 HTML문서 파싱을 중단하고 렌더링 엔진이 **자바스크립트 엔진**에게 제어권을 넘긴다.
2. script 태그의 src 어트리뷰트에 정의된 파일을 서버에 요청한다.
3. 문법적 의미를 갖는 코드의 최소 단위인 **토큰**들로 분해
4. 토큰에 문법적 의미와 구조를 반영한 트리 구조의 자료구조인 **AST**(Abstract Syntax Tree) 생성
5. AST를 기반으로 사람이 이해 해기 쉬운 소스코드와 비교시 덜 추상적이며 더 간결하고 컴퓨터 중심적인 **바이트 코드**로 변환
6. **인터프리터**에 의해 실행

자바스크립트 엔진은 자바스크립트 코드를 파싱하여 cpu가 이해할 수 있는 저수준 언어(바이트코드)로 변환하고 실행하는 역할을 한다.

## 리플로우와 리페인트

DOM은 js코드를 이용해 제어할 수 있도록 DOM API를 제공하는데 js코드를 이용해 사용해서 DOM이나 CSSOM을 변경한 경우 변경된 DOM과 CSSOM은 다시 렌더트리로 결합되고 변경된 렌더트리를 기반으로 레이아웃과 페인트 과정을 거쳐 화면에 렌더링 한다. 이를 리플로우 리페인트라 한다.

<img width="582" alt="스크린샷 2021-11-18 오후 2 04 31" src="https://user-images.githubusercontent.com/87258182/142355318-1031a2c5-d8e1-4d36-8879-c1f370812a30.png">


리플로우 : 레이아웃 계산 
리페인트 : 재결합된 렌더트리 기반 다시 페인트 하는 것

리플로우와 리페인트가 반드시 같이 동작하는것은 아니다. 레이아웃에 영향을 주는 변경이 없는 경우 리페인트만 동작한다.

***


## 정리
 
```javascript
  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8"> //UTF-8로 인코딩
      <link rel="style" href="style.css">//HTML파싱 중단, CSS파일을 파싱 CSSOM생성
    </head>// 다시 HTML 파싱
    <body>
      <h1>Hellow world!</h1>
      // DOM 생성 완료, CSSOM과 결합하여 렌더 트리 생성 -> Layout -> Paint
      <script src="app.js"></script> //JS 파싱 시작 -> AST -> 바이트코드 -> 인터프리터에 의한 실행  
    </body>
  </html>
```

***

> **참조**  
> 
> [모던 자바스크립트 Deep Dive](http://www.yes24.com/Product/Goods/92742567)  
> [poiemWeb](https://poiemaweb.com/js-dom)  
> [위키백과](https://ko.wikipedia.org/wiki/바이트코드)  
