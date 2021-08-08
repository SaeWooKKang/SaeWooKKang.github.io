---
layout: single
title: "Template literals란 ?"
---
## 서론

ES6에서는 기존의 ES5체제에서의 Template strings를 Template literals로 명칭이 변경되었습니다.  

ES5와 ES6를 비교하여 설명해보도록 하겠습니다.


## 1. Template은 문자열이다.
  ``` javascript
  const str1 = `abc`;
  console.log(typeof str1); // string
  ```
### 1-1 문법  

<mark style='background-color: #ffd33d'>ES5</mark>

작은 따옴표 (' ') 혹은 큰 따옴표 (" ")을 사용했습니다.
	
<mark style='background-color: #ffd33d'>ES6</mark>
	
백틱(back-tic) (\` \`)을 사용합니다.


### 1-2 긴 글을 작성 할 경우  
	
<mark style='background-color: #ffd33d'>ES5</mark>  

``` javascript 
var str1 = "첫째줄: 긴 글 입력으로 인한 줄 바꿈\n" + "둘째줄: 긴 글 입력으로 인한 줄바꿈";
console.log(str1); 

//첫째줄: 긴 글 입력으로 인한 줄 바꿈
//둘째줄: 긴 글 입력으로 인한 줄 바꿈
```

<mark style='background-color: #ffd33d'>ES6</mark> 

Template literals을 사용할 경우 문자열 구조를 줄바꿈으로 보다 간단하게 표현 가능합니다.
``` javascript
const str2 = `첫째줄: 긴 글 입력으로 인한 줄 바꿈
둘째줄: 긴 글 입력으로 인한 줄 바꿈`;
	
console.log(str2); 
//첫째줄: 긴 글 입력으로 인한 줄 바꿈
//둘째줄: 긴 글 입력으로 인한 줄 바꿈
```

## 2. 표현식을 사용할 수 있는 문자열이다.  

<mark style='background-color: #ffd33d'>ES5</mark>  
``` javascript
var expression = "표현식";
var  str1 = "문자열작성과 " + expression + " 사용(expression)";
```

<mark style='background-color: #ffd33d'>ES6</mark>  
``` javascript
const expression = `표현식`;
const str2 = `문자열 작성과 ${expression} 사용(expression)`;
```
표현식을 플레이스 홀더인 $(dollar sign) 과  {} 중괄호(curly brace)로 감싸서 작성합니다.
	
\> 문자열과 표현식을 반복해서 작성할 경우 효과적으로 작성할 수 있습니다.

## 3. Tegged templates

함수를 태그(tag)하여 템플릿 리터럴을 실행하면 템플릿 내부에 있는 모든 항목이 태그된 함수의 인수로 제공됩니다.  

예제를 통해 이해 해보도록 하겠습니다.
``` javascript
let ribbonPig = {
    HP: 80,
    MP: 10
};

function tagFunc(string, hp, mp){
    let name = string[0].substr(0, 4);//리본돼지

    return `${name} HP: ${hp} MP: ${mp}`;
} 

let sentence = tagFunc`리본돼지의 체력은 ${ribbonPig.HP}이고 마나는 ${ribbonPig.MP}이다.`;

console.log(sentence);
//리본돼지 HP: 80 MP: 10
```
템플릿은 함수에 첫번째 인자로 **문자열을 배열**로 담는데, 그 구분자 역할을 **표현식**으로 합니다.

  string[0] = '리본돼지의 체력은 '  
  string[1] = '이고 마나는 '  
  string[2] = '이다.'  

두번째 인자부터는 템플릿의 **변수**를 인자로 받습니다.

  ribbonPig 객체에서 정의된 프로퍼티 HP는 80 이므로 함수의 두번째 매개변수 hp는 80  
  ribbonPig 객체에서 정의된 프로퍼티 MP는 10 이므로 함수의 세번째 매개변수 mp는 10


## Reference 

[https://developer.mozilla.org](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Template_literals)

[모던 자바스크립트 핵심 가이드](http://www.yes24.com/Product/Goods/101478466)
