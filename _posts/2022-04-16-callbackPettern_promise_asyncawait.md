# 콜백패턴, 프로미스, async/await의 이해

## 목차
- 콜백패턴 장점
- 콜백패턴 단점
- 프로미스: 비동기 처리 패턴
- async/await 키워드를 활용한 후속처리 메서드 대체

## 콜백패턴의 장점

### 함수는 일급이다

자바스크립트에서 함수는 일급이다. 일급이라는 것은 값으로 다룰 수 있다는 것을 의미한다. 따라서 변수, 함수의 매개변수, 반환값 등 값으로 사용되는 곳은 전부 사용할 수 있다.  

``` javascript
const foo = a => a + 10; // 변수에 할당

const bar = f => f(10); // 매개변수로 전달
bar(foo); // 20

const z = () => foo; // 반환값으로 사용 
```

함수의 일급이라는 성질 덕분에 함수는 보다 유연한 구조로 작성하는 것이 가능해진다.  
다음의 예를 살표보자.  

``` javascript
// 상품 데이터
const products1 = [ 
  {name: 'book', price: 100},
  {name: 'water', price: 200},
  {name: 'cd', price: 300}
];
```
위의 상품 배열에서 상품명, 가격을 뽑기 위해서 함수를 만들어보겠다.  

``` javascript
// 인자로 상품 데이터를 받고 이를 for...of문으로 순회해서 값을 배열에 추가하고 반환하는 함수다.
const pName = products => {
  const res = [];
  for (const product of products) {
    res.push(product.name);
  }
  return res;
};

console.log(pName(products1)) // ['book', 'water', 'cd']
```

``` javascript
// 인자로 상품 데이터를 받고 이를 for...of문으로 순회해서 값을 배열에 추가하고 반환하는 함수다.
const pPrice = products => {
  const res = [];
  for (const product of products) {
    res.push(product.price);
  }
  return res;
};

console.log(pPrice(products1)) // [100, 200, 300]
```

pName과 pPrice는 전체적인 틀은 같다는 것을 알 수 있다. 이렇게 각각 대응되는 함수를 만드는것도 좋지만, 함수의 일급 성질을 이용해서 공통적인 부분을 추상화를 통해 재사용성이 높은 함수를 만들수 있다.    

``` javascript
const map = (f, iter) => {
  const res = [];

  for (const a of iter) {
    res.push(f(a));
  }
  retrun res;
};
```

map 함수를 사용하여 상품명과 가격들을 구하면 다음과 같다.  
``` javascript
const pName = map(p => p.name, products); // ['book', 'water', 'cd']
const pPrice = map(p => p.price, products); // [100, 200, 300]
```

## 콜백함수
콜백함수는 함수의 매개변수를 통해서 **함수 내부로 전달되는 함수**를 의미힌다.  

## 콜백패턴의 단점 
콜백함수는 직접 호출 하는것이 아닌 호출을 위임하게 되는데 직접 호출할 수 없다는것은 리턴 값을 호출자가 사용할 수 없다는것이다.  

``` javascript
setTimeout((a, b) => a + b, 1000, 10, 20);
```

setTimeout 함수는 비동기 함수이고 setTimeout 함수가 실행되면, Web API에 요청을 위임하고 즉시 종료된다. 함수의 인자로 전달된 f 함수는 전역 실행 컨텍스트가 종료되어 콜 스택이 비었을 때, event loop에 의해 콜 스택에 푸시 되어 실행된다. 따라서 함수의 결괏값을 전역 실행 컨텍스트에서 사용할 수 없다. 결괏값을 가지고 추가적인 작업을 하고 싶다면 콜백 패턴으로 함수를 전달해야 한다.  

결괏값을 연속적으로 처리해야 하면 값을 안쪽에서부터 읽어야 해서 가독성에 문제가 생기고 들여쓰기 단계가 깊어져 실수를 유발하게 된다.  

## 프로미스: 비동기 처리 패턴
이렇듯 비동기 처리에서 콜백 패턴은 비동기 처리 결과값을 가지고 후속처리가 까다로운 단점이있다.  

프로미스는 비동기 함수의 처리 상태를 관리하는 객체이고, 값을 처리할 수 있는 후속처리 메서드도 제공한다.  

``` javascript
const promise1 = new Promise(resolve => setTimeout(() => resolve(add(1,2)), 1000));
promise1
  .then(a => a + 100) // 후속 처리 메서드 then
  .then(log);
```

그렇다고 프로미스가 동기적으로 동작한다는것은 아니다.  
프로미스의 후속처리 메서드는 비동기로 동작한다.  

``` javascript
const p1 = new Promise(resolve => setTimeout(()=>resolve(5), 1000));

p1.then(console.log);
console.log('전역 콜스택');

// 출력 순서
// 1. 전역 콜스택 
// 2. 5
```

## 프로미스와 async: await의 조합 

async/await를 조합하면 프로미스의 후속처리 메서드를 사용하지 않고 마치 동기적으로 동작하는 코드를 작성하는 것처럼 코드를 작성할 수 있다.  
async 함수 안의 프로미스 앞에 await 키워드를 사용하면, 프로미스의 상태가 settled가 됐을 때, 처리 결괏값을 프로미스 객체가 아닌 값으로 반환한다.  
``` javascript
const f = async () => {
  let p = await new Promise((a, b) => a + b, 1000, 10, 20);
  console.log(p); // 30

  // (프로미스의 결과값을 값으로 다루는 코드들... )
  p = p - 100; // 후속 처리 메서드를 사용하지 않고 값으로서 다룰수 있다. 
  
  return p;
};

// await 키워드 사용시 async 함수 내부에서는 값으로 다뤄지지만 반환값은 프로미스다. 
const res = f();
console.log(res) // Promise {<fulfilled>: -70}
```

## 정리
비동기 처리시 콜백패턴의 단점은 콜백 헬로 인한 가독성 문제와 복잡도 증가가 있었다. 이러한 배경에서 ES6에서 비동기 처리를 위해 프로미스가 도입되었으며, 프로미스의 후속처리메서드를 활용하여 콜백 헬에 빠지지 않고 읽기 좋은 코드를 작성할 수 있었다. 또한 ES2017에서는 async/await가 도입되어 후속처리 메서드를 사용하지 않고, 프로미스를 값으로서 다룰 수 있게 되었다.  
