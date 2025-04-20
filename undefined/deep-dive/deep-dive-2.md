# 모던 자바스크립트 Deep Dive 스터디 2주차

## 7장 연산자

* 연산자
  * 하나 이상의 _표현&#xC2DD;_&#xC744; 대상으로 산술, 할당, 비교, 논리, 타입, 지수연산 등을 수행해 _하나의 &#xAC12;_&#xC744; 만든다.

### 7.1 산술 연산자

* 산술 연산자는 피연사자를 대상으로 수학적 계산을 수행해 새로운 숫자 값을 만든다.
  * 불가한 경우, NaN을 반환한다.

#### 7.1.2 단항 산술 연산자

* ++, -- 에는 부수효과가 있고, + / - 에는 부수효과가 없다.
  * 증가/감소(++/--) 연산자는 피연사자의 값을 변경하는 부수효과가 있다.
  * 즉, 피연산자의 값을 변경하는 암묵적 할당이 이뤄진다.

```js
let a = 5;
let b = "10";
let c = 3;
let d = "20";

// 부수 효과가 있는 연산자 (++ / --)
a++;       // a는 6이 됨 (부수 효과 O)
--c;       // c는 2가 됨 (부수 효과 O)

// 부수 효과가 없는 연산자 (+ / -)
let numB = +b;   // b는 여전히 "10", numB는 숫자 10 (부수 효과 X)
let negD = -d;   // d는 여전히 "20", negD는 숫자 -20 (부수 효과 X)

console.log("a:", a);       // 6
console.log("c:", c);       // 2
console.log("b:", b);       // "10"
console.log("numB:", numB); // 10
console.log("d:", d);       // "20"
console.log("negD:", negD); // -20
```

### 7.2 할당 연산자

* 할당문은 값으로 평가되는 표현식인 문이다.

```js
var x

console.log(x = 10); // 10


var a, b, c

a = b = c =0;
console.log(a, b, c) // 0 0 0
```

### 7.3 비교 연산자

#### 7.3.1 동등/일치 비교 연산자

```js
NaN == NaN; // false
```

* 일치 비교 연산자에서 주의해야할 것은 NaN이다.
  * 자신과 일치하지 않는 유일한 값이다.
  * Number.isNaN을 사용하자

```js
Number.isNaN(NaN); // true
Number.isNaN(10); // false
Number.isNaN(1 + undefined); // true
```

```js
0 === -0 // true
0 == -0 // true
```

* 숫자 0도 주의가 필요
  * ES6 이후에 도입된 `Object.is` 메서드로 예측 가능한 비교 결과를 반환 외에는 일치 비교 연산자와 동일하게 동작

```js
-0 === +0; // true
Object.is(-0, +0) // false

NaN === NaN; // false
Object.is(NaN, NaN); // true
```

### 7.4 삼항 조건 연산자

* 삼항 조건 연산자 표현식은 값처럼 사용할 수 있지만, if... else 문은 값처럼 사용할 수 없다.

### 7.5 논리 연산자

### 7.6 쉼표 연산자

* 왼쪽 피연산자부터 _차례대로 평&#xAC00;_&#xD558;고, 마지막 피연산자의 평가가 끝나면 _마지막 피연산자의 평가 결과를 반환_

### 7.7 그룹 연산자

* 자신의 피연산자인 표현식을 가장 먼저 평가
* 우선순위 조절 가능

### 7.8 typeof 연산자

* 피연산자의 데이터 타입을 문자열로 반환
  * string
  * number
  * boolean
  * undefined
  * symbol
  * object
  * function

```js
typeof null // object

var foo = null;

typeof foo === null; // false
foo === null; // true
```

* null 타입인지 확인할 때는 일치 연산자(===) 사용해야함

### 7.9 지수연산자

### 7.10 그 외의 연산자

### 7.11 연산자의 부수효과

* 부수효과가 있는 연산자
  * 할당 연산자
  * 증가/감소 연산자
  * delete 연산자

```js
console.log("=== 부수효과 예제 시작 ===");

// 1. 할당 연산자 (=)
let a = 1;
let b;
b = a = 2; // a에 2를 할당하고 그 값을 b에 할당 (부수효과: a와 b의 값이 변경됨)

// 2. 증가/감소 연산자 (++ / --)
let count = 10;

count++;  // 후위 증가 (count = 11)
++count;  // 전위 증가 (count = 12)
count--;  // 후위 감소 (count = 11)
--count;  // 전위 감소 (count = 10)

// 3. delete 연산자
let user = {
  name: "Minsu",
  age: 30,
};

delete user.age; // user 객체에서 age 프로퍼티 삭제 (부수효과: 객체 구조 변경)
```

### 7.12 연산자 우선순위

* 다 기억하는건 의미 없음
  * 다만, 그룹 연산자가 가장 높다는 것 정도는 알고 있자.

### 7.13 연산자 결합 순서

## 8장 제어문

* 조건에 따라 코드 블록을 실행(조건문)하거나 반복 실행(반복문)할 때 사용
* 인위적으로 실행 흐름을 제어하기에, 직관적인 코드의 흐름을 방해한다.
  * forEach, map, filter, reduce 같은 고차함수를 사용한 함수형 프로그래밍 기법에서 복잡성을 해결하려 노력

### 8.1 블록문

### 8.2 조건문

#### 8.2.1 if... else 문

* 조건식이 불리언 값이 아닌 값으로 평가된다면, 암묵적으로 불리언 값으로 강제 변환된다. (9.2절 "암묵적 타입 변환")

### 8.3 반복문

#### 8.3.3. do... while 문

* 코드 블록을 먼저 실행하고 조건식을 평가
* 코드 블록은 무조건 한 번 이상 실행된다.

```js
var count = 0;

do {
	console.log(count); // 0, 1, 2
	count++;
} while (count < 3);
```

### 8.4 break 문

* 아래 코드 블럭을 탈출하기 위해 사용
  * 레이블 문
  * 반복문
  * switch 문
* 레이블 문
  * 식별자가 붙은 문

### 8.5 continue 문

## 9. 타입 변환과 단축 평가

### 9.1 타입 변환이란

* 개발자가 의도적으로 타입을 변환
  * 명시적 타입 변환
  * 타입 캐스팅
* 개발자의 의도와 상관 없이 변환
  * 암묵적 타입 변환
  * 타입 강제 변환
* 타입 변환은 원시 값을 직접 변경하는 것이 아니다.
* 원시 값은 변경 불가능한 값으로 변경할 수 없다.
* 때로는 명시적 타입 변환보다 암묵적 타입 변환이 _가독성 측&#xBA74;_&#xC5D0;서 더 좋을 수 있다.

### 9.2 암묵적 타입 변환

* 자바스크립트 엔진은 표현식을 평가할 때 개발자의 의도와는 상관없이 코드의 문맥을 고려해 암묵적으로 데이터 타입을 강제 변환(암묵적 타입 변환)할 때가 있다.

#### 9.2.1 문자 타입으로 변환

```js
// 문자열과 다른 타입이 더해질 때, 문자열로 변환됨
console.log('Hello ' + 123);     // 'Hello 123'
console.log('Number: ' + true);  // 'Number: true'
console.log('Result: ' + null);  // 'Result: null'

```

#### 9.2.2 숫자 타입으로 변환

```js
console.log('5' * 2);     // 10 ('5' → 숫자로 변환됨)
console.log(true + 1);    // 2   (true → 1)
console.log(false - 1);   // -1  (false → 0)
console.log(null + 1);    // 1   (null → 0)
console.log('' - 1);      // -1  ('' → 0)

// 이건 좀 특이한 경우
console.log(+[]); // 0
console.log(+''); // 0
console.log(+null); // 0
console.log(+false); // 0
```

#### 9.2.3불리언 타입으로 변환

### 9.3 명시적 타입 변환

#### 9.3.1 문자열 타입 변환

* String 생성자 함수를 new 연산자 없이 호출하는 방법
* Object.prototype.toString 메서드를 사용하는 방법
* 문자열 연결 연산자를 이용하는 방법

```js
const value = 123;

// 1. String 생성자 함수를 new 없이 호출
const str1 = String(value);
console.log(str1);           // "123"
console.log(typeof str1);    // "string"

// 2. Object.prototype.toString.call()
const str2 = Object.prototype.toString.call(value);
console.log(str2);           // "[object Number]"

// 3. 문자열 연결 연산자 사용
const str3 = value + '';
console.log(str3);           // "123"
console.log(typeof str3);    // "string"
```

#### 9.3.2. 숫자 타입 변환

* Number 생성자 함수를 new 연산자 없이 호출하는 방법
* parseInt, parseFloat 함수를 사용하는 방법
  * 단항 산술 연산자를 이용하는 방법
  * 산술 연산자를 이용하는 방법

```js
const value = '123.45';

// 1. Number 생성자 함수를 new 없이 호출
const num1 = Number(value);
console.log(num1);           // 123.45
console.log(typeof num1);    // "number"

// 2. parseInt, parseFloat 함수 사용
const num2 = parseInt(value);
console.log(num2);           // 123

const num3 = parseFloat(value);
console.log(num3);           // 123.45

// 3. + 단항 연산자 사용 (unary plus)
const num4 = +value;
console.log(num4);           // 123.45
console.log(typeof num4);    // "number"

// 4. * 산술 연산자 사용 (다른 숫자와 곱함)
const num5 = value * 1;
console.log(num5);           // 123.45
console.log(typeof num5);    // "number"
```

#### 9.3.3 불리언 타입으로 변환

* Boolean 생성자 함수를 new 연산자 없이 호출하는 방법
* ! 부정 논리 연산자를 두 번 사용하는 방법

### 9.4 단축 평가

#### 9.4.1 논리 연산자를 이용한 단축 평가

* 논리합(||) 혹은 논리곱(&&) 연산자 표현식의 평가 결과는 불리언 값이 아닐 수 있다.
* 언제나 2개의 피연산자 중 어느 한쪽으로 평가된다.

```js
// 좌항에서 우항으로 평가된다.
// 'Cat'은 true로 평가된다.
// 그러나 이 시정까지는 표현식을 평가할 수 없다.
// 'Dog'도 true로 평가된다.
// 그래서 'Dog'를 반환한다.

'Cat' && 'Dog' // "Dog"

// 'Cat'에서 이미 true
// 'Dog'는 평가할 필요 없이 "Cat" 반환
'Cat' || 'Dog' // "Cat"
```

<figure><img src="../../.gitbook/assets/스크린샷 2025-04-15 오후 8.22.51.png" alt=""><figcaption></figcaption></figure>

**객체를 가리키기 기대하는 변수가 null 혹은 undefined가 아닌지 확인하고 프로퍼티를 참조할 때**

```js
var elem = null;
var value = elem.value; // TypeError: Cannot read property 'value' of null

var value = elem && elem.value; // null
```

**함수 매개변수에 기본값을 설정할 때**

* 함수를 호출할 때 인수를 전달하지 않으면 매개변수에는 undefined가 할당된다.

```js
function getStringLength(str) {
	str = str || '';
	return str.length
}

// ES6 매개변수의 기본값 설정

function getStringLength(str = '') {
	return str.length
}

```

#### 9.4.2 옵셔널 체이닝 연산자

* ES11에 도입된 옵셔널 체이닝 연산자 ?.는 좌항의 피연산자가 null 또는 undefined인 경우 undefined를 반환하고, 그렇지 않으면 우항의 프로퍼티를 참조한다.

```js
var elem = null;

var value = elem?.value
console.log(value) // undefined
```

이전에는 &&를 사용했음

```js
var elem = null;

var value = elem && elem.value
console.log(value) // null
```

* elem이 Falsy 값이니, 좌항 피연산자가 그대로 반환

#### 9.4.3 null 병합 연산자

* ES11에 도입된 null 병합 연산자 ??
* null 또는 undefined인 경우 피연산자를 반환, 그렇지 않으면 좌항의 피연산자를 반환

```js
// 기본 예시
let userInput = null;
let defaultValue = "기본값";

let result = userInput ?? defaultValue;
console.log(result); // 출력: "기본값"

// undefined인 경우
let name;
let finalName = name ?? "이름 없음";
console.log(finalName); // 출력: "이름 없음"

// falsy 값이지만 null이나 undefined가 아닌 경우
let count = 0;
let resultCount = count ?? 10;
console.log(resultCount); // 출력: 0  (주의! 0은 null이 아니므로 그대로 사용됨)

// 비교: || (OR) 연산자와의 차이
let text = "";
let result1 = text || "기본 텍스트";   // 출력: "기본 텍스트"
let result2 = text ?? "기본 텍스트";   // 출력: ""

console.log(result1); 
console.log(result2);
```

## 10장 객체 리터럴

### 10.1 객체란?

* 자바스크립트는 객체 기반 프로그래밍 언어
* 원시 값을 제외한 나머지 값은 모두 객체 (함수, 배열, 정규 표현식 등)
* 원시 값은 변경 불가능한 값
* 객체 타입 값은 변경 가능한 값
* 자바스크립트에서 사용할 수 있는 모든 값은 프로퍼티 값이 될 수 있음
* 함수는 일급 객체로 값으로 취급 가능 -> 프로퍼티 값으로 가능
  * 메서드라고 부름
* 객체는 객체의 상태를 나타내는 값(프로퍼티)와 프로퍼티를 참조하고 조작할 수 있는 동작(메서드)을 모두 포함할 수 있기 때문에 _상태와 동작을 하나의 단&#xC704;_&#xB85C; 구조화 할 수 있어 유용하다.

### 10.2 객체 리터럴에 의한 객체 생성

* C++이나 자바와 같은 클래스 기반 객체지향 언어는 클래스를 사전에 정의, new 연산자와 함께 생성자를 호출하여 인스턴스를 생성
  * 인스턴스? -> 클래스에 의해 생성되어 메모리에 저장된 실체를 말함
* 다만 자바스크립트는 프로토타입 기반 객체지향 언어로서 객체 생성을 위한 다양한 방법 지원
  * 객체 리터럴
    * 가장 일반적이고, 간단한 방법
  * Object 생성자 함수
  * 생성자 함수
  * Object.create 메서드
  * 클래스 (ES6)
* 객체 리터럴로 객체를 생성하는 예시

```js
var person = {
	name: "Lee",
	sayHello: function() {
		console.log(`Hello! My name is ${this.name}`)
	}
}
```

* 클래스 정의나 new 연산자와 함께 생성자를 호출할 필요가 없음

### 10.3 프로퍼티

* 객체는 프로퍼티의 집합이며, 프로퍼티는 키와 값으로 구성된다.
* 프로퍼티 키는 문자열임으로 따옴표로 묶어야한다.
  * 하지만 식별자 네이밍 규칙을 준수하는 이름인 경우 따옴표 생략 가능

```js
const user = {
  name: "Alice",          // 식별자 네이밍 규칙을 따르므로 따옴표 생략 가능
  age: 30,
  "user-id": 12345,       // 하이픈이 포함되어 있으므로 따옴표 필수
  "1stLogin": true,       // 숫자로 시작하므로 따옴표 필수
  isActive: true,
  address: {
    city: "Seoul",
    "zip-code": "12345"   // 하이픈 포함 → 따옴표 필수
  }
};
```

* 빈 문자열도 키로 사용가능, 하지만 권장하지 않음
* 예약어를 프로퍼티 키로 사용가능, 하지만 권장하지 않음
* 프로퍼티 키를 중복 선언 시 에러없이 덮어씀, 주의

```js
const example = {
  "": "빈 문자열 키",       // 가능하지만 가독성 ↓ → 권장 ❌
  var: "예약어 키",         // 사용은 가능하지만 혼란 초래 가능 → 권장 ❌
  name: "Alice",
  name: "Bob"              // 중복 선언 → 에러 없이 앞의 name 프로퍼티를 덮어씀
};

console.log(example[""]);      // "빈 문자열 키"
console.log(example.var);      // "예약어 키"
console.log(example.name);     // "Bob"

```

### 10.4 메서드

* 자바스크립트의 함수는 객체(일급 객체)다.
  * 함수는 값으로 취급가능
  * 객체의 프로퍼티 값으로 사용할 수 있다.
    * 이를 메서드라고 부른다.

### 10.5 프로퍼티 접근

* 마침표 표기법
* 대괄호 표기법
  * 프로퍼티 키는 반드시 따옴표로 감싼 문자열이어야 한다.

### 10.6 프로퍼티 값 갱신

* 이미 존재하는 프로퍼티에 값을 할당하면 프로퍼티 값이 갱신된다.

### 10.7 프로퍼티 동적 생성

* 존재하지 않는 프로퍼티에 값을 할당하면 프로퍼티가 동적으로 생성되어 추가되고 프로퍼티 값이 할당된다.

### 10.8 프로퍼티 삭제

* delete 연산자는 객체의 프로퍼티를 삭제한다.
* 존재하지 않는 프로퍼티 삭제 시, 아무런 에러 없이 무시된다.

```js
var person = {
	name: "Lee"
}

person.age = 20;

delete person.age;

delete person.address;

console.log(person); // {name: "Lee"}
```

### 10.9 ES6에 추가된 객체 리터럴의 확장 기능

#### 10.9.1 프로퍼티 축약 표현

```js
// ES5

var x = 1, y = 2;
var obj = {
	x: x,
	y: y
}

console.log(obj); // {x: 1, y: 2}
```

```js
// ES6

let x = 1, y = 2;

const obj = { x, y };

console.log(obj); // {x: 1, y: 2}
```

#### 10.9.2 계산된 프로퍼티 이름

* ES5에서 계산된 프로퍼티 이름으로 프로퍼티 키를 동적 생성하려면 객체 리터럴 외부에서 대괄호 표기법을 사용해야 했음
* ES6에서는 객체 리터럴 내부에서도 계산된 프로퍼티 이름으로 프로퍼티 키를 동적 생성 가능

```js
// ES5에서는 객체 리터럴 안에서 i++처럼 계산된 키를 직접 사용할 수 없었음
// 대신 이렇게 해야 했음:

var i = 0;
var obj = {};
obj[i++] = "zero";
obj[i++] = "one";
obj[i++] = "two";

console.log(obj);
// 출력: { '0': 'zero', '1': 'one', '2': 'two' }
```

```js
let i = 0;

const obj = {
  [i++]: "zero",
  [i++]: "one",
  [i++]: "two"
};

console.log(obj); 
// 출력: { '0': 'zero', '1': 'one', '2': 'two' }
```

#### 10.9.3 메서드 축약 표현

```js
// ES6

const obj = {
	name: "Lee",
	sayHi() {
		console.log("HI!" + this.name);
	}
};

obj.sayHi(); // HI! Lee
```

### 11장 원시 값과 객체의 비교

* 원시 값은 변경 불가능한 값이다.
* 객체(참조) 타입의 값은 변경 가능한 값이다.
* 원시 값을 변수에 저장 시, 실제 값이 저장된다.
* 객체는 참조 값이 저장된다.
* 원시 값을 가지는 변수를 다른 변수에 할당하면 원시 값이 복사되어 전달
  * 값에 의한 전달
* 객체를 가리키는 변수를 다른 변수에 할당 시, 참조 값이 복사되어 전달
  * 참조에 의해 전달

### 11.1 원시 값

#### 11.1.1 변경 불가능한 값

* 원시 값은 변경 불가능한 값이다.
  * 읽기 전용(read only) 값으로 변경할 수 없다.
* 변수와 값은 구분해서 생각해야한다.
  * 변수는 값을 저장하기 위해 확보된 메모리 공간 자체, 식별하기 위한 이름이다.
  * 값은 변수에 저장된 데이터로서 표현식이 평가되어 생성된 결과다.
* 변경 불가능한건 변수가 아니라 "값"이다.
  * 변수는 변경(교체)할 수 있다!
* 상수와 변경 불가능한 값은 다르다.
* 상수는 재할당이 금지된 변수일 뿐이다.

```js
const o = {};

o.a = 1;
console.log(o); // {a: 1}
```

* 변수 값을 변경하기 위해 원시 값을 재할당 한다면,
  * 새로운 메모리 공간을 확보,
  * 재할당한 값을 저장,
  * 변수가 참조하던 메모리 공간에 주소를 변경
  * _불변&#xC131;_&#xC774;다.
* 불변성을 갖는 원시 값을 할당한 변수는 재할당 외에 변수 값을 변경할 수 없다.

#### 11.1.2 문자열과 불변성

* 원시 값을 저장하려면 확보해야 하는 메모리 공간의 크기를 결정해야한다.
* 1개의 문자는 2바이트의 메모리 공간에 저장된다.
* 숫자는 무조건 8바이트, 1이나 10000도,

```js
var str = 'Hello';
str = 'world';
```

* 'Hello'와 'world'는 모두 메모리에 존재.
* 식별자 str은 문자열 'Hello'를 가리키다,
* 'world'를 가리키도록 변경되었다.
* 문자열은 유사 배열 객체이며, 이터러블이다.
* 유사 배열 객체
  * 인덱스로 프로퍼티 값에 접근 가능
  * length 프로퍼티를 갖는 객체
  * for문으로 순회 가능

#### 11.1.3 값에 의한 전달

```js
var score = 80;
var copy = score;

console.log(score); // 80
console.log(copy); // 80

score = 100;

console.log(score); // 80
console.log(copy); // ??? == 80
```

* 변수에 원시 값을 갖는 변수를 할당?
  * 할당받는 변수(copy)에는 할당되는 변수(score)의 원시 값이 복사되어 전달
  * _값에 의한 전달_

```js
var score = 80;

var copy = score;

console.log(score, copy) // 80, 80
console.log(score === copy) // true
```

* 같은 숫자 값을 가지지만,
* 80은 다른 메모리 공간에 저장된 별개의 값

<figure><img src="../../.gitbook/assets/스크린샷 2025-04-19 오후 5.17.51.png" alt=""><figcaption></figcaption></figure>

```js
var score = 80;

var copy = score;

console.log(score, copy) // 80, 80
console.log(score === copy) // true

score = 100;

console.log(score, copy) // 100, 80
console.log(score === copy) // false
```

* score 변수와 copy 변수의 값 80은 다른 메모리 공간에 저장된 별개의 값이기에,
* score 변수의 값을 변경해도 copy 값에는 어떠한 영향도 주지 않는다.
* 엄격하게 표현하면 변수에는 값이 아닌 메모리 주소가 전달된다.
* 그러므로 값에 의한 전달도 사실은 값을 전달하는게 아닌, 메모리 주소를 전달한다.
* 전달된 메모리 주소를 통해 메모리 공간에 접근 시 값을 참조 가능

### 11.2 객체

* 객체는 원시 값과 같이 확보해야 할 메모리 공간의 크기를 사전에 정할 수 없다.

#### 11.2.1 변경 가능한 값

* 객체는 변경 가능한 값이다.
* 원시 값을 할당한 변수가 기억하는 메모리 주소를 통해 메모리 공간에 접근하면? 원시 값에 접근할 수 있다.
* 객체는 참조 값에 접근한다. 참조 값은 생성된 객체가 저장된 메모리 공간 주소 그 자체다.

<figure><img src="../../.gitbook/assets/스크린샷 2025-04-19 오후 5.23.50.png" alt=""><figcaption></figcaption></figure>

* 원시 값은 변경 불가능한 값이므로, 원시 값을 갖는 변수의 값을 변경하려면 재할당 밖에 답이 없다.
* 그러나? 객체는 변경 가능한 값이다.
* 객체를 할당한 변수는 재할당 없이 객체를 직접 변경할 수 있다!
  * 재할당 없이 프로퍼티 동적 추가 가능,
  * 갱신 가능,
  * 삭제 가능

**얕은 복사와 깊은 복사**

* 얕은 복사와 깊은 복사로 생성된 객체는 원본과 다른 객체이다.
* 얕은 복사

```js
const original = {
  name: "Alice",
  address: {
    city: "Seoul"
  }
};

const shallow = { ...original }; // 얕은 복사

shallow.address.city = "Busan";

console.log(original.address.city); // "Busan" ← 원본도 바뀜!

```

* 깊은 복사

```js
const original = {
  name: "Alice",
  address: {
    city: "Seoul"
  }
};

const deep = JSON.parse(JSON.stringify(original));

deep.address.city = "Busan";

console.log(original.address.city); // "Seoul" ← 원본은 안 바뀜!

```

#### 11.2.2 참조에 의한 전달

<figure><img src="../../.gitbook/assets/스크린샷 2025-04-19 오후 5.23.50 (2).png" alt=""><figcaption></figcaption></figure>

* 원본 person과 사본 copy 모두 동일한 객체를 _가리킨다._
* \=== 일치 비교 연산자는 변수에 저장되어 있는 값을 타입 변환하지 않고 비교한다.
  * 객체를 할당한 변수는 참조 값을 비교하고,
  * 원시 값을 할당한 변수는 원시 값을 비교한다.

```js
var person1 = {
	name: 'Lee'
}

var person2 = {
	name: 'Lee'
}

console.log(person1 === person2) // false
console.log(person1.name === person2.name) // true
```

## 12장 함수

### 12.1 함수란?

* 일련의 과정을 문으로 구현하고, 코드 블록으로 감싸서 하나의 실행 단위로 정의한 것

<figure><img src="../../.gitbook/assets/스크린샷 2025-04-20 오후 5.08.28.png" alt=""><figcaption></figcaption></figure>

### 12.2 함수를 사용하는 이유

* 코드의 재사용 측면
* 중복을 제거, 재사용성 향상
  * 유지보수의 편의성, 코드의 신뢰성
* 코드의 가독성

### 12.3 함수 리터럴

* 자바스크립트의 함수는 객체 타입의 값
* 함수도 함수 리터럴로 생성할 수 있다.

```js
var f = function add(x, y) {
	return x + y;
}
```

* 일반 객체와 다르다. 일반 객체는 호출할 수 없지만, 함수는 호출할 수 있다.

### 12.4 함수 정의

* 함수를 정의하는 방법
  * 함수 선언문
  * 함수 표현식
  * Function 생성자 함수
  * 화살표 함수(ES6)

#### 12.4.1 함수 선언문

```js
function greet(name) {
  return `Hello, ${name}!`;
}

console.log(greet('Alice')); // Hello, Alice!
```

* 함수 리터럴과 동일한 형태
  * 함수 리터럴은 함수 이름을 생략 가능, 하지만 함수 선언문은 함수 이름 생략 불가능
  * 함수 선언문 = 문, 표현식이 아니다. => undefined 반환
* 호이스팅의 영향을 받는다.

#### 12.4.2 함수 표현식

```js
const greet = function(name) {
  return `Hello, ${name}!`;
};

console.log(greet('Bob')); // Hello, Bob!
```

* 자바스크립트에서 함수는 객체 타입의 값
  * 함수는 값처럼 변수에 할당할 수 있고, 프로퍼티 값이 될 수 있고, 배열의 요소가 될 수 있다.
  * 함수가 일급객체이다.

#### 12.4.3 함수 생성 시점과 함수 호이스팅

* 함수 선언문으로 정의한 함수와 함수 표현식으로 정의한 함수 생성 시점이 다르다.
* 함수 선언문도 런타임 이전에 자바스크립트 엔진에 의해 실행된다.
  * 런타임에는 이미 함수 객체가 생성되어 있고, 함수 이름과 동일한 식별자 할당까지 완료된 상태
  * _함수 호이스팅_
* 함수 호이스팅과 변수 호이스팅은 미묘하게 다르다.
  * var 키워드로 선언된 변수는 undefined로 초기화된다.
  * 함수 선언문으로 정의한 함수를 함수 선언문 이전에 호출하면 함수 호이스팅에 의해 호출 가능
* 함수 표현식은 변수에 할당되는 값이 함수 리터럴인 문
* 따라서 변수 선언문과 변수 할당문을 한 번에 축약 표현과 동일하게 동작
  * 런타임에 평가되므로, 함수 표현식의 함수 리터럴도 할당문이 실행되는 시점에 평가되어 함수 객체가 된다.
* _함수 표현식으로 함수 정의시, 함수 호이스팅 발생 X, 변수 호이스팅이 발생._

#### 12.4.4. Function 생성자 함수

```js
const greet = new Function('name', 'return `Hello, ${name}!`;');

console.log(greet('Charlie')); // Hello, Charlie!
```

* 일반적이지 않으며 바람직하지 않은 방법
* Function 생성자 함수로 생성한 함수는 클로저를 생성하지 않는 등, 다르게 동작한다.

#### 12.4.5 화살표 함수

```js
const greet = (name) => {
  return `Hello, ${name}!`;
};

// 또는 더 간단하게 (return 생략 가능)
const greetShort = name => `Hello, ${name}!`;

console.log(greet('Dana'));       // Hello, Dana!
console.log(greetShort('Eve'));   // Hello, Eve!
```

* 화살표 함수는 항상 익명함수로 정의한다.
* this 바인딩 방식이 다르고,
* prototype 프로퍼티가 없으며
* argument 객체를 생성하지 않는다.

### 12.5 함수 호출

#### 12.5.1 매개변수와 인수

* 함수 외부에서 함수 내부로 전달할 필요가 있을 경우, 매개변수를 통해 인수를 전달
* 인수는 값으로 평가될 수 있는 표현식이어야한다.
* 인수는 개수와 타입에 제한이 없다.
* 함수 몸체 내부에서 변수와 동일하게 취급
  * 함수가 호출되면 일반 변수와 마찬가지로 undefined로 초기화 된 이후 인수가 순서대로 할당된다.
* 매개변수는 함수 몸체 내부에서만 참조 가능, 외부에서는 참조 불가
* 매개변수의 스코프는 함수 내부다.

```js
function add(x, y) {
	console.log(x, y)
	return x + y;
}

add(2, 5);

console.log(x, y); // ReferenceError: x is not defined
```

* 매개변수의 개수와 인수 개수가 일치하는지 체크하지 않는다.
  * 인수가 부족해서 인수가 할당되지 않은 매개변수 값은 undefined다.
  * 많으면 무시된다.
* 모든 인수는 암묵적으로 arguments 객체의 프로퍼티로 보관된다.

```js
function add(x, y) {
	console.log(argementes);
	// Arguments(3) [2,5,10, callee f, Symbol(Symbol.iterator): f]
	return x + y;
}

add(2, 5, 10);
```

#### 12.5.2 인수 확인

* 함수 몸체에서 typeof 로 타입을 확인하고
* 개수를 확인할 수 있음

#### 12.5.3 매개변수의 최대 개수

* ECMAScript 사양에서는 최대 개수에 대해 명시적으로 제한하고 있지 않다.
* 물리적 한계는존재.
* 이상적인 함수는 한 가지 일만 해야하며 가급적 작게 만들어야한다.
* 최대 3개 이상을 넘지 않을 것을 권장

#### 12.5.4 반환문

* return 키워드와 표현식(반환값)으로 반환문을 이용해 실행 결과를 함수 외부로 반환 가능
* return의 역할
  * 함수 실행을 중단하고 함수 몸체를 빠져나간다.
  * return 뒤에 오는 표현식을 평가해 반환한다.
    * 명시적으로 지정하지 않으면 undefined 반환

### 12.6 참조에 의한 전달과 외부 상태의 변경

```js
function changeVal(primitive, obj) {
	primitive += 100;
	obj.name = 'Kim';
}

var num = 100;
var person = { name: 'Lee' };

console.log(num); // 100
console.log(person); // {name: "Lee"}

changeVal(num, person);

// 원시 값은 원본이 훼손되지 않는다.
console.log(num); // 100

// 객체는 원본이 훼손된다.
console.log(person); // {name: "Kim"}
```

* 원시 값은 변경 불가능한 값 -> 재할당을 통해 할당된 원시 값을 새로운 원시 값으로 교체
* 객체는 변경 가능한 값 -> 직접 변경

<figure><img src="../../.gitbook/assets/스크린샷 2025-04-20 오후 5.42.54.png" alt=""><figcaption></figcaption></figure>

* 객체에 경우 원본 객체가 변경되는 '부수효과'가 발생
* 불변 객체로 만드는 방법이 좋다.
* 방어적 복사를 이용하거나,
* 깊은 복사를 이용할 수 있다.
* 순수함수를 통해 부수효과를 최대한 억제하며 오류를 피하는 프로그래밍 패러다임
* 함수형 프로그래밍

### 12.7 다양한 함수의 형태

#### 12.7.1 즉시 실행 함수

```js
(function () {
  console.log('즉시 실행 함수!'); // 출력: 즉시 실행 함수!
})();
```

* 한 번만 호출되며 다시 호출할 수 없다.
* 익명 함수를 사용하는 것이 일반적
  * 기명 즉시 실행 함수도 사용 가능
* 반드시 그룹 연산자(...) 로 감싸야한다.

#### 12.7.2 재귀 함수

```js
function factorial(n) {
  if (n === 1) return 1;
  return n * factorial(n - 1);
}

console.log(factorial(5)); // 출력: 120
```

* 함수가 자기 자신을 호출하는 것을 재귀 호출이라 한다.
* 재귀 함수는 재귀 호출을 수행하는 함수다.
* 탈출 조건을 반드시 만들어야한다.
  * 탈출 조건이 없으면 스택 오버플로가 발생
* 가끔 반복문을 사용하는 것보다 재귀 함수를 사용하는게 더 직관적일 수 있다.
  * 한정적으로 사용 필요

#### 12.7.3 중첩 함수

```js
function outer(name) {
  function inner() {
    return `Hello, ${name}!`;
  }
  return inner();
}

console.log(outer('Minsu')); // 출력: Hello, Minsu!
```

#### 12.7.4 콜백 함수

```js
function repeat(n, callback) {
  for (let i = 0; i < n; i++) {
    callback(i);
  }
}

repeat(3, function(i) {
  console.log(`반복: ${i}`);
});
// 출력:
// 반복: 0
// 반복: 1
// 반복: 2
```

* 자바스크립트 함수는 일급 객체이므로 함수의 매개변수에 함수를 전달 가능
* _콜백 함수_
  * 함수의 매개변수를 통해 다른 함수의 내부로 전달되는 함수
  * 매개 변수를 통해 함수 외부에서 콜백 함수를 전달받은 함수롤 _고차함&#xC218;_&#xB77C;고 한다.

#### 12.7.5 순수 함수와 비순수 함수

```js
// 순수 함수
function add(a, b) {
  return a + b;
}

// 비순수 함수
let count = 0;
function increase() {
  return ++count;
}

console.log(add(2, 3)); // 5 (항상 동일)
console.log(increase()); // 1 (외부 상태에 의존)
console.log(increase()); // 2

```

* 부수 효과가 없는 함수를 순수 함수라고 한다.
  * 동일한 인수가 전달되면 언제나 동일한 값을 반환한다.
  * 외부 상태를 변경하지 않는다.

## 13장 스코프

### 13.1 스코프란?

* 스코프란 식별자가 유효한 범위를 말한다.

### 13.2 스코프의 종류

* 코드는 전역과 지역으로 구분할 수 있다.

#### 13.2.1 전역과 전역 스코프

* 전역 변수는 어디서든지 참조할 수 있다.

#### 13.2.2 지역과 지역 스코프

* 지역이란 함수 몸체 내부를 말한다.
* 지역은 지역 스코프를 만든다.
* 지역 변수는 자신의지역 스코프와 하위 지역 스코프에서 유효하다.

### 13.3 스코프 체인

* 함수는 전역에서도, 함수 몸체 내부에서도 정의할 수 있다.
* 함수는 중첩될 수 있으므로, 함수의 지역 스코프도 중첩될 수 있다.
* 변수를 참조할 때 자바스크립트 엔진은 스코프 체인을 통해 변수를 참조하는 코드의 스코프에서 시작하여 _상위 스코프 방향으로 선언된 변수를 검&#xC0C9;_&#xD55C;다.
  * _상위 스코프에서 선언한 변수를 하위 스코프에서 참조할 수 있다._

#### 13.3.1 스코프 체인에 의한 변수 검색

* 상위 스코프에서 유효한 변수는 하위 스코프에서 자유롭게 참조할 수 있지만 하위 스코프에서 유효한 변수를 상위 스코프에서 참조할 수 없다.

#### 13.3.2 스코프 체인에 의한 함수 검색

### 13.4 함수 레벨 스코프

* 코드 블록이 아닌 함수에 의해서 지역 스코프가 만들어 진다.
* var 키워드로 선언된 변수는 오로지 함수의 코드 블록만을 지역 스코프로 인정한다.
* ES6 에서 도입된 let, const는 블록 레벨 스코프를 지원

### 13.5 렉시컬 스코프

* 자바스크립트는 렉시컬 스코프를 따른다.
* 함수를 어디서 호출 X / 함수를 어디서 정의했는지에 따라 상위 스코프를 결정한다.
* 호출된 위치는 상위 스코프 결정에 어떠한 영향을 주지 않는다.
  * 함수의 상위 스코프는 언제나 자신이 정의된 스코프다.

## 14장 전역 변수의 문제점

### 14.1 변수의 생명 주기

#### 14.1.1 지역 변수의 생명 주기

* 함수 내부에서 선언된 지역 변수는 함수가 호출되면 생성되고 종료하면 소멸
* 전역 변수 한정, 변수 선언은 런타임 이전 단계에 먼저 실행
* 함수 내부 선언 변수는 함수 호출 직후 함수 몸체 코드 실행 전에 실행된다.
* 호이스팅은 스코프 단위로 동작한다.

```js
var x = 'global'

function foo() {
	console.log(x); // undefined
	var x = 'local';
}

foo();
console.log(x); // 'global'
```

#### 14.1.2 전역 변수의 생명 주기

* var 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 된다.

### 14.2 전역 변수의 문제점

* 암묵적 결합
  * 모든 코드가 전역 변수를 참조하고 변경할 수 있다.
  * 변수의 유효 범위가 크면 클수록 가독성은 나빠지고, 위험성도 높아진다.
* 긴 생명 주기
  * 메모리 리소스를 오랜 기간 소ㅂ한다.
* 스코프 체인 상에서 종점에 존재
  * 전역 변수의 검색 속도가 제일 느리다.
* 네임스페이스 오염

### 14.3 전역 변수의 사용을 억제하는 방법

* 전역 변수를 반드시 사용해야 할 이유를 찾지 못했다면 지역 변수를 사용해야 한다.
* 변수의 스코프는 좁을수록 좋다.

#### 14.3.1 즉시 실행 함수

```js
(function () {
  const msg = 'Hello from IIFE';
  console.log(msg);
})();

console.log(typeof msg); // undefined

```

#### 14.3.2 네임스페이스 객체

```js
const MyApp = {}; // 전역 변수는 이 객체 하나만!

MyApp.user = {
  name: 'Minsu',
  greet: function () {
    console.log(`Hello, ${this.name}`);
  }
};

MyApp.user.greet(); // Hello, Minsu
```

#### 14.3.3 모듈 패턴

* 즉시 실행 함수로 감싸 하나의 모듈을 만든다.
  * 클로저 기반으로 동작
  * 캡슐화가 가능
    * 정보 은닉

```js
const Counter = (function () {
  let count = 0; // private 변수

  return {
    increase() {
      return ++count;
    },
    decrease() {
      return --count;
    }
  };
})();

console.log(Counter.increase()); // 1
console.log(Counter.decrease()); // 0
// console.log(count); // ❌ Error: count is not defined
```

#### 14.3.4 ES6 모듈

* ES6 모듈을 사용하면 전역 변수를 사용할 수 없다.
* 모듈 스코프를 제공한다.

```html
<script type="module" src="lib.mjs"></script>
<script type="module" src="app.mjs"></script>
```

* 구형 브라우저에서 동작하지 않으며
* 트랜스파일링이나 번들링이 필요
* Webpack 등의 모듈 번들러를 사용하는 것이 일반적

```js
// utils.js
export function sayHello(name) {
  console.log(`Hello, ${name}`);
}

```

```js
// main.js
import { sayHello } from './utils.js';

sayHello('Minsu'); // Hello, Minsu

```

## 15장 let, const 키워드와 블록 레벨 스코프

### 15.1 var 키워드로 선언한 변수의 문제점

#### 15.1.1 변수 중복 선언 허용

#### 15.1.2 함수 레벨 스코프

* for 문 변수는 전역 변수가 된다..?!
* 전역 변수를 남발할 가능성을 높인다.

#### 15.1.3 변수 호이스팅

* 가동성을 떨어뜨리고
* 오류 발생 여지를 남긴다.

<figure><img src="../../.gitbook/assets/스크린샷 2025-04-20 오후 6.18.01.png" alt=""><figcaption></figcaption></figure>

### 15.2. let 키워드

#### 15.2.1 변수 중복 선언 금지

#### 15.2.2 블록 레벨 스코프

* 함수, if문, for문, while문, try/catch문 을 지역 스코프로 인정하는 _블록 레벨 스코&#xD504;_&#xB97C; 따른다.

#### 15.2.3 변수 호이스팅

* 변수 호이스팅이 발생하지 않는 _것처럼_ 동작

```js
console.log(foo); // ReferenceError: foo is not defined
let foo;
```

* var 키워드로 선언한 변수는 런타임 이전에 자바스크립트 엔진에 의해 "선언 단게"와 "초기화 단계"가 한번에 진행
* let 키워드로 선언한 변수는 "선언 단게"와 "초기화 단계"가 분리되어 진행
  * 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 선언 단계가 먼저 실행되지만,
  * 초기화 단계는 변수 선언문에 도달했을 때 실행

<figure><img src="../../.gitbook/assets/스크린샷 2025-04-20 오후 6.17.39.png" alt=""><figcaption></figcaption></figure>

* 초기화 이전에 접근 시 참조 에러가 발생
  * 스코프 시작부터 초기화 시작 지점까지 변수 참조가 불가한 영역을 "일시적 사각지대(TDZ)"라고 한다.
* _그러다 여전히 호이스팅은 발생한다._

```js
let foo = 1; // 전역 변수

{
	console.log(foo); // ReferenceError: Cannot access 'foo' before initialization
	let foo = 2; // 지역 변수
}
```

* 호이스팅이 발생하기에 전역변수 foo를 출력하지 못한다.
* 자바스크립트는 ES6에서 도입된 let, const를 포함해 모든 선언(var, let, const, function, function\*, class 등)을 호이스팅 한다.

#### 15.2.4 전역 객체와 let

* let 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다.

### 15.3 const 키워드

#### 15.3.1 선언과 초기화

* const 키워드로 선언한 변수는 반드시 선언과 동시에 초기화해야 한다.

#### 15.3.2 재할당 금지

#### 15.3.3 상수

* 원시 값을 할당한 경우 변수 값을 변경할 수 없다.
  * 원시 값은 재할당 없이는 값을 변경할 수 없기 때문

#### 15.3.4 const 키워드와 객체

* const 키워드로 선언된 변수에 객체를 할당한 경우 값을 변경 가능
* const 키워드는 재할당을 금지할 뿐 _'불변'을 의미하지 않는다._

### 15.4 var vs. let vs. const

* ES6를 사용한다면 var 키워드는 사용하지 않는다.
* 재할당이 필요한 경우에 한정, let을 사용한다. 이때 변수의 스코프는 최대한 좁게
* 나머지? const
