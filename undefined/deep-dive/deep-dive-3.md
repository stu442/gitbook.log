# 모던 자바스크립트 Deep Dive 스터디 3주차

## 18장 함수와 일급 객체

### 18.1 일급 객체

* 다음 조건을 만족하는 객체는 _일급 객&#xCCB4;_&#xB2E4;.
  * 무명의 리터럴로 생성할 수 있다. 즉, 런타임에 생성이 가능하다
  * 변수나 자료구조(객체, 배열 등)에 저장할 수 있다.
  * 함수 매개변수에 전달할 수 있다.
  * 함수의 반환값으로 사용할 수 있다.
* 자바스크립트 함수는 위 조건 모두 만족
* 함수 = 일급 객체 = 객체와 동일하게 사용 가능 = 객체 = 값
* 함수형 프로그래밍이 가능!

### 18.2 함수 객체의 프로퍼티

#### 18.2.1 arguments 프로퍼티

* 함수 객체의 arguments 프로퍼티 값은 arguments 객체다.
  * 함수 호출 시 전달된 인수들의 정보를 담고 있는 순회 가능한 유사 배열 객체이며, 함수 내부에서 지역 변수처럼 사용된다.
  * 함수를 정의할 때 선언한 매개변수는 함수 몸체 내부에서 변수와 동일하게 취급
  * 함수가 호출되면 함수 몸체 내에서 암묵적으로 매개변수가 선언, undefined로 초기화 후 인수가 할당된다.
* arguments 객체는 매개변수 개수를 확정하라 수 없는 "가변 인자 함수"를 구현할 때 유용하다.

```js
function sum() {
  let total = 0;

  for (let i = 0; i < arguments.length; i++) {
    total += arguments[i];
  }

  return total;
}

console.log(sum(1, 2, 3));     // 6
console.log(sum(10, 20, 30, 40)); // 100
console.log(sum());            // 0

```

* arguments 객체는 배열 형태지만, 실제 배열이 아닌 유사 배열 객체다.
  * 유사 배열 객체란, length 프로퍼티를 가진 객체로, for 문으로 순회할 수 있다.
* 유사 배열 객체는 배열이 아니므로 배열 메서드를 사용할 시 에러 발생
  * 간접 호출을 사용해야함
* ES6에서는 Rest 파라미터를 도입

```js
function sum(...args) {
	return args.reduce((pre, cur) => pre + cur, 0);
}

console.log(sum(1,2)) // 3
console.log(sum(1,2,3,4,5)) // 15
```

#### 18.2.2 caller 프로퍼티

* 비표준 프로퍼티로, 앞으로도 표준화될 예정이 없는 프로퍼티
* 참고로만 알아두자.

#### 18.2.3 length 프로퍼티

* 함수를 정의할 때 선언한 매개변수의 개수를 가리킨다.
  * arguments 객체의 length는 _인&#xC790;_&#xC758; 개수,
  * 함수 객체의 length 프로퍼티는 _매개 변&#xC218;_&#xC758; 개수

#### 18.2.4 name 프로퍼티

* ES6에서 정식 표준
  * ES5와 ES6 기준 다른 동작
  * 익명 함수 표현식에서 ES5의 name은 빈 문자열
  * ES6에서는 함수 객체를 가리키는 식별자

```js
// ES5 (구버전 브라우저)
var oldFunc = function() {};

console.log(oldFunc.name); // "" (빈 문자열) -- ES5에서는 익명 함수는 name이 비어있었어

// ES6 (모던 자바스크립트)
const newFunc = function myFunctionName() {};

console.log(newFunc.name); // "myFunctionName" -- ES6에서는 함수 이름을 식별자로 추론해줘
```

#### 18.2.5 \_\_proto\_\_ 접근자 프로퍼티

* 모든 객체는 \[\[Prototype]]이라는 내부 슬롯을 갖는다.
  * 객체지향 프로그래밍의 상속을 구현하는 프로토타입 객체를 가리킨다.
* 내부 슬롯에 직접 접근 불가하여 \_\_proto\_\_ 접근자를 통해 간접적으로 접근 가능

#### 18.2.6 prototype 프로퍼티

* constructor 만이 소유하는 프로퍼티다.
  * 일반 객체와 생성자 함수로 호출할 수 없는 non-constructor에는 prototype 프로퍼티가 없다.

## 19장 프로토타입

* 자바스크립트는 명령형, 함수형, 프로토타입 기반 객체지향 프로그래밍을 지원하는 멀티 패러다임 프로그래밍 언어다.
* 자바스크립트를 이루는 거의 "모든 것"이 객체다.
  * 원시 값을 제외한 나머지 값은 모두 객체다.

### 19.1 객체지향 프로그래밍

* 객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임
  * 실세계의 실체를 인식하는 사고를 프로그래밍에 접목하려는 시도
* 객체의 "상태"를 나타내는 _데이&#xD130;_&#xC640; 상태 데이터를 조작할 수 있는 "동작"을 하나의 논리적 단위로 묶어 생각한다.
* 각 객체는 고유의 기능을 갖는 독립적인 부품으로 볼 수 있지만,
* 다른 객체와 관계성을 가질 수 있다.

### 19.2 상속과 프로토타입

* 상속은 객체지향 프로그래밍의 핵심 개념
  * 어떤 객체의 프로퍼티 혹은 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것

```js
// 생성자 함수
function Circle(radis) {
	this.radius = radius;
}

// 프로토타입에 추가한다.
Circle.prototype.getArea = function () {
	return Math.PI * this.radius ** 2;
}

// 인스턴스 생성
const circle1 = new Circle(1);
const circle2 = new Circle(2);

// 프로토타입 Circle.prototype으로부터 getArea 메서드를 상속받는다.
// 즉, Circle 생성자 함수가 생성하는 모든 인스턴스는 하나의 getArea 메서드를 공유
console.log(circle1.getArea === circle2.getArea); true

console.log(circle1.getArea()) // 3.141592~~
console.log(circle2.getArea()) // 12.56637~~
```

* 프로토타입을 사용하면 인스턴스를 아무리 생성해도 하나의 메서드만을(prototype.getArea)를 사용하게 된다.
  * 중복의 제거

### 19.3 프로토타입 객체

* 프로토타입 객체란 상속을 구현하기 위해 사용된다.
* 모든 객체는 \[\[Prototype]]이라는 내부 슬롯을 가진다.
* 프로토타입은 객체 생성 방식에 의해 결정된다.
* 모든 객체는 하나의 프로토타입을 갖는다.
* 모든 프로토타입은 생성자 함수와 연결되어 있다.

#### 19.3.1 \_\_proto\_\_ 접근자 프로퍼티

* 모든 객체는 \_\_proto\_\_ 접근자 프로퍼티를 통해 자신의 프로토타입, \[\[ProtoType]] 내부 슬롯에 간접적으로 접근할 수 있다.
* \_\_proto\_\_ 접근자 프로퍼티는 상속을 통해 사용된다.
  * Object.prototype의 프로퍼티다.
* \_\_proto\_\_ 접근자 프로퍼티를 통해 프로토타입에 접근하는 이유
  * 상호 참조에 의해 프로토타입 체인이 생성되는 것을 방지하기 위함
  * 프로토타입 체인은 단방향 링크드 리스트로 구현되어야한다.
  * 순환 참조하게 되면, 프로토타입 체인 종점이 존재하지 않게된다.
    * 무한 루프가 된다!!
* \_\_proto\_\_ 접근자 프로퍼티를 코드 내에서 직접 사용하는 것은 권장하지 않는다.
  * \_\_proto\_\_ 접근자 프로퍼티는 ES5까지 사양에 포함되지 않은 비표준이었다.
  * 모든 객체가 \_\_proto\_\_ 접근자 프로퍼티를 사용할 수 있는 것이 아니다.
  * 그래서 권장하지 않는다.

#### 19.3.2. 함수 객체의 prototype 프로퍼티

* 함수 객체만이 소유하는 prototype 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다.

#### 19.3.3 프로토타입의 constructor 프로퍼티와 생성자 함수

> 모든 프로토타입은 constructor 프로퍼티를 갖는다.

### 19.4 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입

> 리터럴 표기법에 의해 생성된 객체의 경우 프로토타입의 constructor 프로퍼티가 가리키는 생성자 함수가 반드시 객체를 생성한 생성자 함수라고 단정할 수 없다.

```js
// obj 객체는 Object 생성자 함수로 생성한 객체가 아니라 객체 리터럴로 생성했다.
const obj = {};

// 하지만 obj 객체의 생성자 함수는 Object 생성자 함수다.
console.log(obj.constructor === Object); //true
```

* 객체 리터럴로 객체를 만들었더라도, 그 객체는 `Object.prototype`을 상속받습니다.
* `Object.prototype`에는 `constructor`라는 프로퍼티가 있는데, 이는 원래 `Object` 생성자 함수를 가리킵니다.
* 그래서 객체 리터럴로 만든 객체의 `constructor`를 확인하면 `Object` 생성자 함수를 가리키게 됩니다.

### 19.5 프로토타입의 생성 시점

* 프로토타입은 생성자 함수가 생성되는 시점에 생성된다.
  * 사용자 정의 생성자 함수
  * 빌트인 생성자 함수

#### 19.5.1 사용자 정의 생성자 함수와 프로토타입 생성 시점

> 생성자 함수로서 호출할 수 있는 함수, 즉 constructor는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.

> 생성자 함수로서 호출할 수 없는 함수, 즉 non-constructor는 프로토타입이 생성되지 않는다.

```js
// 함수 선언문은 constructor입니다.
function Person(name) {
  this.name = name;
}

console.log(typeof Person.prototype); // "object"


const ArrowFunc = () => {};

console.log(ArrowFunc.prototype); // undefined
```

#### 19.5.2 빌트인 생성자 함수와 프로토타입 생성 시점

> 모든 빌트인 생성자 함수는 전역 객체가 생성되는 시점에 생성된다. 생성된 프로토타입은 빌트인 생성자 함수의 prototype 프로퍼티에 바인딩된다.

### 19.6 객체 생성 방식과 프로토타입의 결정

* 객체 리터럴
* Object 생성자 함수
* 생성자 함수
* Object.create 메서드
* 클래스(ES6)

> 각 방식마다 세부적인 객체 생성 방식의 차이는 있으나 추상 연산 OrdinaryObjectCreate에 의해 생성된다는 공통점이 있다.

### 19.7 프로토타입 체인

> 프로토타입 체인이란?\
> 자바스크립트는 객체의 프로퍼티에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티가 없다면 \[\[Prototype]] 내부 슬롯의 참조를 따라 자신의 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색한다. 프로토타입 체인은 자바스크립트가 객체지향 프로그래밍의 상속을 구현하는 매커니즘이다.

* 프로토타입의 종점은 Object.prototype 이다.

### 19.8 오버라이딩과 프로퍼티 섀도잉

### 19.9 프로토타입의 교체

> 프로토타입은 임의의 다른 객체로 변경할 수 있다. 다시 말해, 부모 객체인 프로토타입을 동적으로 변경할 수 있다. `생성자 함수` 또는 `인스턴스`에 의해 교체할 수 있다.

### 19.10 instanceof 연산자

> `객체 instance of 생성자 함수`\
> 우변의 생성자 함수의 prototype에 바인딩된 객체가 좌변의 객체의 프로토타입 체인 상에 존재하면 true or false

### 19.11 직접 상속

#### 19.11.1 Object.create에 의한 직접 상속

```javascript
let obj = Object.create(Object.prototype, {
  x : { value:1, writable: true, enumerable:true, configurable:true}
});
```

#### 19.11.2 객체 리터럴 내부에서 \_\_proto\_\_에 의한 직접 상속

```javascript
const myProto = {x:10};
const obj = {
  y:20,
  __proto__:myProto;
};

console.log(Object.getPrototypeOf(obj) === myProto); //true
```

### 19.13 프로퍼티 존재 확인

> `in 연산자` 혹은 `Obejct.prototype.hasOwnProperty` 메서드 사용

### 19.14 프로퍼티 열거

> for (변수선언문 in 객체) {...}\
> 순회 대상 객체의 프로퍼티뿐만 아니라 상속받은 프로토타입의 프로퍼티까지 열거한다.

```javascript
for(key in Person){
  //검색
}
```

## 21장 빌트인 객체

### 21.1 자바스크립트 객체의 분류

* 표준 빌트인 객체
  * ECMAScript 사양에 정의된 객체
  * 실행 환경에 관계없이 언제나 사용 가능
* 호스트 객체
  * 실행 환경이 제공에서 추가로 제공하는 객체
  * 브라우저에선 DOM, BOM, Canvas
  * Node.js 에서는 고유 API
* 사용자 정의 객체
  * 사용자가 직접 정의한 객체

### 21.2 표준 빌트인 객체

* Object, String, Number, Boolean, Symbol, Date, Math, RegExp, Array, Map/Set, WeakMap/WeakSet, Function, Promise, Reflect, Proxy, JSON, Error 등 40여개의 표준 빌트인 객체를 제공
* Math, Reflect, JSON을 제외한 표준 빌트인 객체는 모두 인스턴스를 생성할 수 있는 생성자 함수 객체다.

### 21.4 전역 객체

* 코드 실행 이전에 자바스크립트 엔진에 의해 가장 먼저 생성되는 특수한 객체
* 브라우저에서 window, Node.js에서 global

## 22장 this

### 22.1 this 키워드

* 객체는 상태를 나타내는 프로퍼티와 동작을 나타내는 메서드를 하나의 논리적인 단위로 묶은 복합적인 자료구조
* 객체 리터럴 방식으로 생성한 객체의 경우, 메서드 내부에서 메서드 자신이 속한 객체를 가리키는 식별자를 재귀적으로 참조 가능
* JS의 this는 함수가 호출되는 방식에 따라 바인딩 될 값이 달라진다...!!

### 22.2 함수 호출 방식과 this 바인딩

* 일반 함수 호출
  * 전역 객체가 바인딩 된다.

```js
function showThis() {
  console.log(this);
}
showThis(); // 브라우저: window / Node.js: global 또는 undefined (strict mode)
```

* 메서드 호출
  * 메서드를 호출한 객체가 바인딩 된다.

```js
const obj = {
  name: "minsu",
  showThis() {
    console.log(this);
  }
};
obj.showThis(); // { name: "minsu", showThis: [Function: showThis] }
```

* 생성자 함수 호출
  * 생성자 함수가 생성할 인스턴스에 바인딩 된다.

```js
const obj = {
  name: "minsu",
  showThis() {
    console.log(this);
  }
};
obj.showThis(); // { name: "minsu", showThis: [Function: showThis] }
```

* Function.prototype.apply/call/bind 메서드에 의한 간접 호출
  * 메서드에 첫번째로 인수를 전달한 객체에 바인딩 된다.

```js
function sayHello() {
  console.log(this.name);
}
const user = { name: "minsu" };

sayHello.call(user);   // "minsu"
sayHello.apply(user);  // "minsu"
const bound = sayHello.bind(user);
bound();               // "minsu"

```
