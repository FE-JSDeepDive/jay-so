## let, const 키워드와 블록 레벨 스코프

---

### 1. var 키워드로 선언한 변수의 문제점

#### 변수 중복 선언 허용

var 키워드로 선언한 변수는 중복 선언이 가능함
- 중복되어 선언이 가능하기에 먼저 선언된 변수의 값을 변경하는 문제점 발생

var 키워드 중복 예시
- x변수와 y변수 중복 선언
  - 변수 초기화 후, 변수에 값의 재할당이 없는 문은 무시됨 
```JavaScript
var x = 1;
var y = 1;

//var키워드로 선언된 변수는 같은 스코프 내에서 중복 선언이 가능함
var x = 100;

//값의 재할당이 없는 문은 var키워드가 없는 것처럼 동작함
var y;

console.log(x); //100
console.log(y); //1
```
<br/>

#### 함수 레벨 스코프

- var 키워드로 선언된 변수가 오직 함수의 코드 블록안에서만 지역 스코프로 동작하고 다른 모든 코드 블록(if,for)에서는 지역 스코프가 적용되지 않는 특성

-> 함수 레벨의 스코프는 전역 변수를 남발할 가능성이 높으며, 의도치 않게 전역 변수가 중복 선언됨

예시
1. 전역 변수 선언 
   - var x = 1
2. if문 내 선언
    - if 블록 내부의 x는 전역 변수 x의 값을 재할당
```JavaScript
var x = 1;

if(true){
    //var로 선언된 변수는 전역 변수, 이미 선언된 변수 x가 있으므로 변수의 값을 변경함
    var x = 10; 
}

console.log(x); //10
```

for문 예시
```JavaScript
var i = 10;

for(var i = 0; i < 5; i++){
    console.log(i); //0 1 2 3 4
}

console.log(i); //5
```

#### 변수 호이스팅

- var 키워드로 변수 선언 시 호이스팅에 의해 변수 선언이 먼저 실행됨
- 이후 값 할당으로 인해 var 키워드 변수의 값이 변경됨

-> 프로그램의 흐름상 맞지 않아 가독성을 떨어트리고 오류를 발생시킬 여지를 남김

```JavaScript
console.log(foo); //undefined

foo = 123;

console.log(foo); //123

var foo; //변수 선언은 JS엔진에 의해 런타임 이전에 호이스팅됨
```
<br/>

### 2.let 키워드

#### 변수 중복 선언 금지

var 키워드
- 중복된 변수명 재사용 가능 -> 해당 변수의 값을 재할당

let 키워드
- 중복된 변수명 재사용 불가 -> 에러 발생

```JavaScript
//var 키워드는 중복된 변수명을 사용 가능함
var foo = 123;
var foo = 456;

//let 키워드는 중복된 변수명 사용시 에러가 발생됨
let bar = 123;
let bar = 456; //SyntaxError: Identifier 'bar' has already been decleared
```
<br/>

#### 블록 레벨 스코프
var 키워드
- 함수 레벨 스코프 = 함수 코드 블록만 지역 스코프로 인정 

let 키워드
- 블록 레벨 스코프 = 모든 코드 블록(함수, if문, for문, while문, try/catch문 등)을 지역 스코프로 인정

```JavaScript
let foo = 1; //전역 변수

{
    let foo = 2; //지역 변수 
    let bar = 3; //전역 변수
}

console.log(foo); //1
console.log(bar); //RefenceError
```
<br/>

#### 변수 호이스팅

var 키워드
- JS엔진에 의해 변수가 암묵적으로 선언과 초기화가 한번에 진행됨
  - var 키워드로 값을 할당하지 않아도 undefined가 할당됨
  - 선언 단계에서 스코프(실행 컨텍스트 - 렉시컬 환경)에 변술을 등록하여 JS엔진에 변수의 존재를 알람
  - 초기화 단계에서 undefined로 변수를 초기화함
  - 변수 선언문 이전에 접근해도 에러가 발생하지 않고 undefined를 반환함

let 키워드
- JS엔진에 의해 암묵적으로 선언단계가 이루어지지만, 초기화는 이루어지지 않음(변수 선언과 초기화 단게가 분리되어 진행됨)
  - let 키워드로 변수가 초기화 되기 이전에 접근할 수 없어 ReferenceError(참조 에러)가 발생함
  - 일시적 사각지대(Temporal Dead Zone:TDZ) = 스코프의 시작 지점부터 초기화 시작 지점까지 변수를 참조할 수 없는 구간

```JavaScript
console.log(foo); //ReferenceError
let foo;
```
<br/>

```JavaScript
//var 키워드로 선언한 변수는 런타임 이전에 선언단계외 초기화 단계가 실행됨 -> 변수 선언문 이전에 변수를 참조할 수 잇음

console.log(foo); //undefined

var foo;
console.log(foo); //undefined

foo = 1;
console.log(foo); //1
```
<br/>


```JavaScript
//let 키워드도 런타임 이전에 선언 단계가 실행됨
//초기화 이전의 일시적 사각지대에서는 변수를 참조할 수 없음
console.log(foo); //RefenceError

let foo; //변수 선언문에서 초기화 단계가 실행됨
console.log(foo);

foo = 1;
console.log(foo);
```
<br/>

let 키워드도 변수 호이스팅이 발생됨
- let 키워드 = 블록 레벨 스코프 
- 호이스팅이 발생되지 않는 것처럼 보이지만 호이스팅됨
1. let 전역 변수 선언 및 값 할당됨
2. 블록 레벨의 let의 foo가 선언되어 호이스팅됨
3. 값 할당 후 참조 가능

```JavaScript
let foo = 1; //전역 변수

{
    //해당 let 키워드가 호이스팅되지 않으면 전역 변수의 값을 사용함
    console.log(foo); //ReferenceError
    let foo = 2;
}
```
<br/>

#### 전역 객체와 let
var 키워드
- 선언된 전역 변수와 전역 함수, 선언하지 않는 변수에 값을 할당하는 암묵적 전역은 전역 객체 window의 프로퍼티가 됨
- 전역 객체의 프로퍼티를 참조할 때 window를 생략할 수있음

암묵적 전역 =  변수 선언 키워드(`var`, `let`, `const`) 없이 식별자에 값을 할당할 때 발생하는 현상

```JavaScript
//전역 변수
var x = 1;

//암묵적 전역
y = 2;

//전역 함수
function foo() {}

//var키워드로 선언한 전역 변수 = 전역 객체 window의 프로퍼티
console.log(window.x); //1

//전역 객체 window의 프로퍼티 = 전역 변수처럼 사용할 수 있음
console.log(x);

//암묵적 전역 = 전역 객체 window의 프로퍼티
console.log(window.y); //2
console.log(y); //2

//함수 선언문으로 정의한 전역 함수 = 전역 개체 window의 프로퍼티
console.log(window.foo); //f foo() {}

//전역 객체 window의 프로퍼티 = 전역 변수처럼 사용할 수 있음
console.log(window.foo); 
```
<br/>

let 키워드 
- let키워드로 선언한 전역 변수 = 전역 객체의 프로퍼티가 아님
  - window.foo와 같이 접근할 수 없음

```JavaScript
let x = 1;

//let, const 키워드로 선언한 전역 변수 = 전역 객체 window의 프로퍼티가 아님
console.log(window.x); //undefined
console.log(x); //1
```
<br/>

### 3. const 키워드

#### 선언과 초기화

const 키워드로 선언한 변수는 반드시 선언과 동시에 초기화해야 함
- 초기화하지 않을 경우 문법 에러가 발생함

```JavaScript
const foo = 1;

const foo; //SyntaxError
```
<br/>

const 키워드로 선언한 변수는 블록 레벨 스코프를 가짐

```JavaScript
{
  console.log(foo); //ReferenceError
  const foo = 1;
  console.log(foo); //1
}

//블록 레벨의 스코프를 가짐으로 다음은 에러가 발생함
console.log(foo); //ReferenceError
```
<br/>

#### 재할당 금지

const 키워드로 선언한 변수는 재할당이 금지됨
```JavaScript
const foo = 1;
foo = 2; //TypeError
```
<br/>

#### 상수

const 키워드로 선언한 변수는 재할당이 금지됨으로 상수라고 불림
- 원시 타입의 경우 const키워드로 선언한 변수는 변경할 수 없음
- 상태 유지와 가독성, 유지보수의 편의를 위해 사용됨
- 일반적으로 상수의 이름은 대문자로 선언하며 언더 스코어(_)로 구분하여 스네이크 케이스로 표시함

```JavaScript
//변수 이름을 대문자로 선언하여 상수임을 나타냄
//세율
const TAX_RATE = 0.1;

//세전 가격
let preTaxPricw = 100;

//세후 가격
let afterTaxPrice = preTaxPrice + (preTaxPrice * TAX_RATE);

console.log(afterTaxPrice); //110
```
<br/>

#### const 키워드와 객체

const 키워드로 선언된 변수에 객체를 할당한 경우 값을 변경할 수 있음
- const 키워드는 재할당을 금지할 뿐 불변을 의미하지 않음

```JavaScript
const person = {
  name: 'Lee'
};

//객체 = 변경이 가능한 값, 재할당 없이 변경이 가능함
person.name = 'Kim';

console.log(person); //{name:"Kim"}
```
<br/>

### 4.var vs let vs const

변수 사용 시 기본적으로 const를 사용하고, 이후 재할당이 필요할 때 let 키워드를 사용하는 것이 좋음

const 키워드 사용 시 의도치 않은 재할당을 방지함

- ES6 사용 시 var 키워드를 사용하지 않음
- 재할당이 필요한 경우에 한해 let 키워드를 사용함
  - 변수의 스코프는 최대한 좁게 만듬
- 변경이 발생하지 않고 읽기 전용으로 사용하는(재할당이 필요 없는 상수) 원시 값과 객체는 const 키워드를 사용함
- const 키워드는 재할당을 금지함으로 var,let 키워드보다 안전함