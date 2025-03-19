## this

---

### 1. this 키워드

객체의 구성

- 상태를 나타내는 프로퍼티
- 동작을 나타내는 메서드

객체의 메서드는 객체의 상태(프로퍼티)를 참조하고 변경할 수 있어야 함

- 이때 메서드가 자신이 속한 객체의 프로퍼티를 참조하려면 먼저 자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 함
- 객체 리터럴 방식으로 생성한 객체의 경우, 메서드 내부에서 객체를 가리키는 식별자를 재귀적으로 참조할 수 있음

객체 리터럴 방식

- getDiameter 메서드 내에서 객체 자신인 circle을 참조하고 있음
- 해당 참조 표현식이 평가되는 시점은 getDiameter 메서드가 호출되어 함수 몸체가 실행되는 시점임
- getDiameter 메서드가 호출 시점에는 이미 객체 리터럴의 평가가 완료되어 객체가 생성되었고 circle 식별자에 생성된 객체가 할당된 이후임으로 circle 식별자를 참조할 수 있음
- 자기 자신이 속한 객체를 재귀적으로 참조하는 방식은 일반적이지 않으며 바람직하지도 않음

```javascript
const circle = {
  //프로퍼티: 객체 고유의 상태
  radius: 5,

  //메서드
  getDiameter() {
    //자신의 객체인 circle을 참조함
    return 2 * circle.radius;
  },
};

console.log(circle.getDiameter()); //10
```

생성자 방식으로 인스턴스를 생성하는 경우

- 생성자 함수 내부에는 프로퍼티 또는 메서드를 추가하기 위해 자신이 생성할 인스턴스를 참조할 수 있어야 함
- 생성자 함수에 의한 객체 생성은 먼저 생성자 함수를 정의한 후 new 연산자와 함께 생성자 함수를 호출하는 단계가 추가로 필요함(생성자 함수로 인스턴스를 생성하려면 먼저 생성자 함수가 존재해야 함)
- 생성자 함수를 정의하는 시점은 아직 인스턴스를 생성하기 이전임으로 생성자 함수가 생성할 인스턴스를 가리키는 식별자를 알 수 없음

-> 이에 JS에서 this 키워드를 사용함

```javascript
fuction Circle(radius){
    //해당 시점에는 생성자 함수 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없음
    ????.radius = radius;
}

Circle.prototype.getDiameter = function(){
    //해당 시점에는 생성자 함수 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없음
    return 2 * ????.radius;
};

//생성자 함수로 인스턴스를 생성하려면 먼저 생성자 함수를 정의해야 함
const circle = new Circle(5);
```

this 키워드

- 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수임
- this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있음
- this는 JS 엔진에 의해 암묵적으로 생성되며, 코드 어디서든 참조할 수 있음
- 함수를 호출하면 arguments 객체와 this가 암묵적으로 함수 내부에 전달됨
- 함수 내부엣 arguments 객체를 지역 변수처럼 사용할 수 있는 것처럼 this도 지역변수 처럼 사용할 수 있음
- this가 가리키느 값(this 바인딩)은 함수 호출 방식에 의해 동적으로 결정됨

this 바인딩

- 바인딩 : 식별자와 값을 연결시키는 과정
- this바인딩은 this키워드와 this가 가리킬 객체를 바인딩함

this 키워드를 이용한 객체 리터럴

- 객체 리터럴의 메서드 내부에서는 this 메서드를 호출한 객체를 가키림

```javascript
//객체 리터럴
const circle = {
  radius: 5,
  getDiameter() {
    //this는 메서드를 호출한 객체를 가리킴
    return 2 * this.radius;
  },
};

console.log(circle.getDiameter()); //10
```

this키워드를 이용한 생성자 함수

- 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킴

```javascript
//생성자 함수
function Circle(radius) {
  //this는 생성자 함수가 생성할 인스턴스를 가리킴
  this.radius = radius;
}

Circle.prototype.getDiameter = function () {
  //this는 생성자 함수가 생성할 인스턴스를 가리킴
  return 2 * this.radius;
};

//인스턴스 생성
const circle = new Circle(5);
console.log(circle.getDiameter()); //10
```

JS는 this함수가 호출되는 방식에 따라 this에 바인딩될 값이 동적으로 결정됨

this는 코드 어디서든 참조가 가능함

this 키워들를 사용한 전역 코드

- this는 객체의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수임으로 일반적으로 객체의 메서드 내부 또는 생성자 함수 내부에서만 의미가 있음
- strict mode가 적용된 일반 함수 내부에서는 this에는 undefined가 바인딩됨

```javascript
//this는 어디서나 참조 가능함
//전역에서 this는 전역 객체window를 참조함
console.log(this); //window

function square(number) {
  //일반 함수 내부에서 this는 전역 객체 window를 가리킴
  console.log(this); //window
  return number * number;
}
square(2);

const person = {
  name: "Lee",
  getName() {
    //메서드 내부의 this는 메서드를 호출함 ㄴ객체를 의미함
    console.log(this); //{name: "Lee", getName: f}
    return this.name;
  },
};
console.log(person.getName()); //Lee

function Person(name) {
  this.name = name;
  //생성자 함수 내부에서 this는 생성자 함수가 생성할 인스턴스를 가리킴
  console.log(this); //Person {name: "Lee"}
}

const me = new Person("Lee");
```

<br/>

### 2. 함수 호출 방식과 this 바인딩

this 바인딩(this에 바인딩될 값)은 함수 호출 방식, 즉 함수가 어떻게 호출되었는지에 따라 동적으로 결정됨

렉시컬 스코프와 this 바인딩은 결정 시기가 다름

- 함수의 상위 스코프를 결정하는 방식인 렉시컬 스코프는 함수 정의가 평가되어 함수 객체가 생성되는 시점에 상위 스코프를 결정함
- this 바인딩은 함수 호출 시점에 결정됨

주의해야 할 점

- 동일한 함수도 다양한 방식으로 호출할 수 잇음

1. 일반 함수 호출
2. 메서드 호출
3. 생성자 함수 호출
4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출

```javascript
//this 바인딩은 함수 호출 방식에 따라 동적으로 결정됨
const foo = functin () {
  console.dir(this);
};

//동일한 함수도 다양한 방식으로 호출할 수 있음

//1. 일반 함수 호출
// foo 함수를 일반적인 방식으로 호출, foo 함수 내부의 this는 전역 객체 window를 가리킴
foo(); //window

//2. 메서드 호출
//foo 함수를 프로퍼티 값으로 할당하여 호출, foo 함수 내부의 this는 메서드를 호출한 객체 obj를 가리킴
const obj = {foo};
obj.foo(); //obj

//3. 생성자 함수 호출
// foo 함수를 new 연산자와 함께 생성자 함수로 호출, foo 함수 내부의 this는 생성자 함수가 생성한 인스턴스를 가리킴
new foo(); //foo {}

//4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출
//foo 함수 내부의 this는 인수에 의해 결정됨
const bar = {name: 'bar'};

foo.call(bar); //bar
foo.apply(bar); //bar
foo.bind(bar)(); //bar
```

<br/>

#### 일반 함수 호출

기본적으로 this에는 전역 객체가 바인딩됨

```javascript
function foo() {
  console.log("foo's this: ", this); //window

  function bar() {
    console.log("bar's this: ", this); //window
  }
  bar();
}
foo();
```

전역 함수, 중첩 함수를 일반 함수로 호출하면 함수 내부의 this에는 전역 객체가 바인딩됨

- this는 객체의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수임으로 객체를 생성하지 않는 일반 함수에서 this는 의미가 없음

strict mode가 적용된 일반함수

- 내부의 this에는 undefined가 바인딩됨

```javascript
function foo() {
  "use strict";

  console.log("foo's this: ", this); //undefined

  function bar() {
    console.log("bar's this:", this); //undefined
  }
  bar();
}
foo();
```

메서드 내에 정의된 중첩 함수도 일반 함수로 호출되면 중첩 함수 내부의 this에는 전역 객체가 바인딩됨

```javascript
//var 키워드로 선언한 전역 변수 value는 전역 객체의 프로퍼티임
var value = 1;
//단, const 키워드로 선언한 전역 변수 value는 전역 객체의 프로퍼티가 아님
//const value = 1;

const obj = {
  value: 100,
  foo() {
    console.log("foo's this: ", this); //{value: 100, foo: f}
    console.log("foo's this.value: ", this.value); //100

    //메서드 내에서 정의한 중첩 함수
    function bar() {
      console.log("bar's this: ", this); //window
      console.log("bar's this.value: ", this.value); //1
    }

    //메서드 내에서 정의한 중첩 함수도 일반 함수로 호출되면 중첩 함수 내부의 this에는 전역 객체가 바인딩됨
    bar();
  },
};

ob.foo();
```

콜백 함수가 일반 함수로 호출된다면 콜백 함수 내부의 this에도 전역 객체가 바인딩됨

- 어떠한 함수라도 일반 함수로 호출되면 this에 전역 객체가 바인딩됨

```javascript
var value = 1;

const obj = {
  value: 100,
  foo() {
    console.log("foo's this: ", this); //{value: 100, foo: f}
    //콜백 함수 내부의 this에는 전역 객체가 바인딩됨
    setTimeout(function () {
      console.log("callback's this:", this); //window
      console.log("callback's this.value: ", this.value); //1
    }, 100);
  },
};

obj.foo();
```

일반 함수로 호출된 모든 함수(중첩 함수, 콜백 함수 포함) 내부의 this는 전역 객체가 바인딩됨

- 중첩 함수 또는 콜백 함수는 외부 함수를 돕는 헬퍼 함수의 역할을 함으로 외부 함수의 일부 로직을 대신하는 경우가 대부분임

메서드 내부의 중첩 함수나 콜백 함수의 this 바인딩을 메서드의 this 바인딩과 일치시키기 위한 방법

```javascript
var value = 1;

const obj = {
  value: 100,
  foo() {
    //this 바인딩(obj)을 변수 that에 할당함
    const that = this;

    //콜백 함수 내부에서 this 대신 that을 참조함
    setTimeout(function () {
      console.log(that.value); //100
    }, 100);
  },
};

obj.foo();
```

위 방법 이외에도 JS는 this를 명시적으로 바인딩할 수 있는 Function.prototype.apply, Function.prototype.call, Function.prototype.bind메서드를 제공함

```javascript
var value = 1;

const obj = {
  value: 100,
  foo() {
    //콜백함수에 명시적으로 this를 바인딩함
    setTimeout(
      function () {
        console.log(this.value); //100
      }.bind(this),
      100
    );
  },
};

obj.foo();
```

화살표 함수를 이용한 this 바인딩

```javascript
var value = 1;

const obj = {
  value: 100,
  foo() {
    //화살표 함수 내부의 this는 상위 스코프의 this를 가리킴
    setTimeout(() => console.log(this.value), 100); //100
  },
};

obj.foo();
```

<br/>

### 모던 자바스크립트 Deep Dive 22장: `this`

#### **메서드 호출**

메서드 내부의 `this`는 메서드를 호출한 객체에 바인딩됩니다. 이는 메서드를 소유한 객체가 아니라, 메서드 이름 앞의 마침표(`.`) 연산자 앞에 기술된 객체를 기준으로 결정됩니다.

```javascript
const person = {
  name: "Lee",
  getName() {
    // 메서드 내부의 this는 메서드를 호출한 객체에 바인딩된다.
    return this.name;
  },
};

// 메서드 getName을 호출한 객체는 person이다.
console.log(person.getName()); // Lee
```

- 위 예제에서 `getName` 메서드는 `person` 객체의 프로퍼티로 정의되었지만, 메서드 호출 시 `this`는 `person` 객체를 참조합니다.
- 하지만, 메서드를 다른 객체에 할당하거나 일반 함수로 호출하면 `this`는 달라질 수 있습니다.

```javascript
const anotherPerson = {
  name: "Kim",
};

anotherPerson.getName = person.getName;

// getName 메서드를 호출한 객체는 anotherPerson이다.
console.log(anotherPerson.getName()); // Kim

const getName = person.getName;

// 일반 함수로 호출된 경우, this는 전역 객체를 참조한다.
console.log(getName()); // Node.js 환경에서는 undefined, 브라우저에서는 ''
```

#### **생성자 함수 호출**

생성자 함수 내부의 `this`는 새로 생성된 객체를 가리킵니다. 생성자 함수는 주로 객체를 초기화하는 데 사용되며, `new` 키워드와 함께 호출됩니다.

```javascript
function Person(name) {
  this.name = name;
}

const alice = new Person("Alice");
console.log(alice.name); // Alice
```

- 생성자 함수 호출 시, `this`는 자동으로 새로 생성된 객체에 바인딩됩니다.

#### **Function.prototype.apply/call/bind 메서드에 의한 간접 호출**

`apply`, `call`, `bind` 메서드는 함수 호출 시 `this`를 명시적으로 설정할 수 있습니다.

1. **apply**

   - 첫 번째 인자로 `this`를 설정하고, 두 번째 인자로 배열 형태의 인수를 전달합니다.

   ```javascript
   function greet(greeting) {
     console.log(`${greeting}, ${this.name}`);
   }

   const person = { name: "Alice" };
   greet.apply(person, ["Hello"]); // Hello, Alice
   ```

2. **call**

   - 첫 번째 인자로 `this`를 설정하고, 나머지 인자를 직접 전달합니다.

   ```javascript
   greet.call(person, "Hi"); // Hi, Alice
   ```

3. **bind**

   - 새로운 함수를 반환하며, 반환된 함수의 `this`는 고정됩니다.

   ```javascript
   const boundGreet = greet.bind(person);
   boundGreet("Hey"); // Hey, Alice
   ```
