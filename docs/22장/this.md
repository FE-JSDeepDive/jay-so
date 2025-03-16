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

#### 일반 함수 호출

#### 메서드 호출

#### 생성자 함수 호출

#### Function.prototype.apply/call/bind메서드에 의한 간접 호출
