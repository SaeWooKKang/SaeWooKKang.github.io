---
layout: single
title: "생성자 함수란 ?"
---
# 생성자 함수란?

예를 들어서 RPG게임에는 다양한 몬스터들이 있을것이다. 이를 객체로 정의하면 다음과 같다.

<img src ="https://user-images.githubusercontent.com/87258182/127734932-8bbc3192-98f5-458d-9667-c520d52f0526.png" width="50%" height="50%">

몬스터들에게는 name, HP, power등 다양한 공통된 속성이 있다.

게임에서 몬스터들을 공통된 속성들을 일일이 작성한다면 효율적이지 못할것이다.

이러한 객체들의 중복된 코드 작성을 해결해주는 것이 생성자 함수이다. 

__생성자__ 란 구체적인 instance(monsterA)를 만들기 위한 __'틀'__ 로서 자바스크립트에서는  
new 키워드와 함께 함수를 호출함으로서 함수가 생성자 역할을 한다.

이 '__틀__(함수)'에 공통된 속성들을 넣어서 정의한다.

* * *

 ### 예시

* * *

사람의 공통된 속성을 가지고 구체적인 instance인 'cheolsu'를 만들어 보겠다.

<img src="https://user-images.githubusercontent.com/87258182/127735155-d1f4d144-6a91-475d-bd95-8379ea7afaff.png" width="50%" height="50%">

익명함수를 변수 Person에 담았고,

new 키워드와 함께 함수를 호출해 함수를 생성자 함수로서 호출하였다. 

생성자 함수를 호출할 때 this는 구체적 instance인 cheolsu를 가르킴으로 

<img src="https://user-images.githubusercontent.com/87258182/127735222-143c28a7-feb6-41ce-bf35-7f568e4cb606.png" width="50%" height="50%">

크롬 개발자도구에서 cheolsu를 호출해보면 

<img src="https://user-images.githubusercontent.com/87258182/127736459-432da466-bb79-4ac9-9fe7-dd670880210e.png" width="50%" height="50%">

cheolsu의 생성자 함수인 'Person'이 보이고 그 아래 프로퍼티로 age와 name이 설정된 것을 볼 수 있다.

* * *

 ### 정리

* * *

이처럼 객체들을 정의해야 할 때 공통된 속성들을 함수에 정의해 놓고 생성자 함수를 실행해 
인스턴스를 정의한다면 코드를 간소화 시킬수 있고, 인스턴스 객체의 속성 추가, 유지, 보수등 편리한 이점이 있다.



