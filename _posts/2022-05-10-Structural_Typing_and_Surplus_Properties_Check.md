# 구조적 탸이핑과 잉여 속성 체크

### 공통 코드
``` javascript
interface Person {
  name: string;
  age: number;
}
const getName = (p: Person) => p.name;
```

## 구조적 타이핑 

- JS는 덕 타이핑 기반이고 TS는 JS를 모델링 한다. 
- TS는 매개변수가 요구사항만 만족하면 어떤 타입이던 신경 쓰지 않는다. 

``` javascript
// p3의 타입은 { name: string; age: number; grade: string; }이다.
const p3 = { name: 'name3', age: 30, grade: 'high' };

// getName 함수의 매개변수 타입 Person은 p3 타입의 부분 집합이다(구조가 호환된다).
getName(p3); // 가능

// 객체를 직접 할당할 경우는 '잉여 속성 체크'가 동작히므로 불가능
getName({ name: 'name3', age: 30, grade: 'high' }); 
```




## 잉여 속성 체크

- 객체를 변수에 할당할 경우 잉여 속성 체크 동작한다. 
- 지정된 타입의 속성만 할당할 수 있음.

``` javascript
const p1: Person = { name: 'name1', age: 10 }; // 가능

// 잉여 속성 체크 동작 -> grade는 Person 타입 속성에 없음
// Person 타입에 정의되지 않은 속성 할당 불가
const p2: Person = { name: 'name2', age: 20, grade: 'middle' };

getName({ name: 'name1', age: 10 }) // 가능

// 잉여 속성 체크 동작 -> grade는 Person 타입 속성에 없음
getName({ name: 'name1', age: 10, grade: 'middile' }) // 불가능
```

## 정리 

- TS는 JS 동작을 모델링하므로 덕타이핑 기반이고 따라서 구조적 타이핑을 사용한다.
- 변수에 객체 할당시 임시 변수에 객체를 담은 변수를 할당하면 '잉여 속성 체크'를 건너뛰고 구조적 타이핑만 동작한다. 


> **참고**
> 이펙티브 타입스크립트: 아이템 4, 아이템 11
