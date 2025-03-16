## Strict mode

---

### 1.strict mode란?

예시

```javascript
function foo() {
  x = 10;
}
foo();

console.log(x); //10
```

foo함수 내에서 선언하지 않은 x변수에 10값을 할당함

- JS엔진은 x 변수가 어디서 선언되었는지 스코프 체인을 통해 검색함
- foo함수 내에는 존재x (스코프 체인을 통해 상위 스코프에서 찾음)
- 전역 스코프에서도 x변수의 선언이 존재하지 않기 때문에 ReferenceError를 발생할 것 같지만, JS엔진이 암묵적으로 전역 객체에 x 프로퍼티를 동적 생성함

-> 전역 객체의 x 프로퍼티는 마치 전역 변수 처럼 사용할 수 있음

-> 해당 현상을 암묵적 전역이라고 함

암묵적 전역

- 오타 등 여러 이유로 발생할 수 있음
- 오류를 발생시키는 원인이 될 가능성이 높음(반드시 var, let, const 키워드를 사용하여 변수를 선언한 다음 사용해야 함)

ES5부터 strict mode(엄격 모드)가 추가됨

- strict mode = JS의 문법을 좀 더 엄격히 적용하여 오류를 발생시킬 가능성이 높거나 JS 엔진의 최적화 작업에 문제를 일으킬 수 있는 코드에 대해 명시적인 에러를 발생 시킴

ESLint와 같은 린트 도구를 사용해도 strict mode와 유사한 효과를 얻을 수 있음

- 린트 도구 = 정적 분석 기능을 통해 소스코드를 실행하기 전에 소스코드를 스캔하여 문법적 오류뿐만 아닌 잠재적 오류까지 찾아내고 오류의 원인을 서포팅함
  <br/>

### 2. strict mode의 적용

strict mode를 적용하려면 전역의 선두 또는 함수 몸체의 선두에 'use strict';를 추가함

전역에 'use strict'를 추가한 예시

- 전역의 선두에 추가하면 스크립트 전체에 적용됨

```javascript
"use strict";

function foo() {
  x = 10; //ReferenceError: x is not defined
}
foo();
```

함수 몸체의 선두에 'use strict'를 추가한 예시

```javascript
function foo() {
  "use strict";

  x = 10; //ReferenceError: x is not defined
}
foo();
```

잘못된 'use strict' 사용 예시

- 코드의 선두에 'use strict';를 위치시키지 않으면 strict mode가 정산 작동하지 않음

```javascript
function foo() {
  x = 10; //에러를 발생시키지 않음
  ("use strict");
}
foo();
```

<br/>

### 3. 전역에 strict mode를 적용하는 것은 피하자

전역에 적용한 strict mode는 스크립트 단위로 적용됨

- 스크립트 단위로 적용된 strict mode는 다른 스크립트에 영향을 주지 않고 해당 스크립트에 한정되어 적용됨

```html
<!DOCTYPE html>
<body>
    <script>
        'use strict';
    </script>
    <script>
        x = 1; //에러가 발생하지 않음
        console.log(x); //1
    </script>
    <script>
        'use strict';

        y = 1; //ReferenceError
    </script>
</body>
</html>
```

strict mode 스크립트와 non-strict mode는 혼용은 오류를 발생 시킬 수 있음

- 외부 서드파티 라이브러리를 사용하는 경우 non-strict mode인 라이브러리도 있음으로 전역에 strict mode를 적용하는 것은 바람직하지 않음
- 이럴 경우 즉시 실행 함수로 스크립트 전체를 감싸 스코프를 구분하고 즉시 실행 함수의 선두에 strict mode를 적용함

```javascript
//즉시 실행 함수의 선두에 strict mode 적용
(function (){
    'use strict';

    ....
})

```

<br/>

### 4. 함수 단위로 strict mode를 적용하는 것도 피하자

함수에 strict mode와 non-strict node를 혼용하는 것은 혼란을 주며 모든 함수에 strict mode를 적용하는 것은 번거로움

또한 strict mode가 적용된 함수가 참조할 함수 외부의 컨텍스트에 strict mode를 적용하지 않는다면 문제가 발생할 수 있음

```javascript
(function () {
  //non-strict mode
  var let = 10; //에러가 발생하지 않음

  function foo() {
    "use strict";

    let = 20; //에러 발생
  }
  foo();
})();
```

따라서 strict mode는 즉시 실행 함수로 감싼 스크립트 단위로 적용하는 것이 바람직함
<br/>

### 5. strict mode가 발생시키는 에러

strict mode를 적용했을 때 발생하는 에러들의 예시

#### 암묵적 전역

선언하지 않은 변수를 참조하면 ReferenceError 발생

```javascript
(function () {
  "use strict";

  x = 1;
  console.log(x); //ReferenceError
})();
```

#### 변수, 함수, 매개변수의 삭제

- delete 연산자로 변수, 함수, 매개변수를 삭제하면 SyntaxError가 발생함

```javascript
(function () {
  "use strict";

  var x = 1;
  delete x; //SyntaxError

  function foo(a) {
    delete a; //SyntaxError
  }
  delete foo; //SyntaxError
})();
```

#### 매개변수 이름의 중복

- 중복된 매개변수 이름을 사용하면 SyntaxError가 발생함

```javascript
(function () {
  "use strict";

  //SyntaxError: Duplicate parameter name
  function foo(x, x) {
    return x + x;
  }
  console.log(foo(1, 2));
})();
```

#### with 문의 사용

- with문을 사용 시 SyntaxError가 발생함
- with문은 전달된 객체를 스코프 체인에 추가함

with문

- 동일한 객체의 프로퍼티를 반복해서 사용할 때 객체 이름을 생략할 수 있어 코드가 간단해지는 효과가 있음
- 성능과 가독성이 나빠지는 문제가 있어 사용하지 않는 것이 좋음

```javascript
(function (){
    'use strict';

    //SyntaxError: strict mode code may not inclued a with statement
    with({x:1}{
        console.log(x);
    })
})
```

<br/>

### 6. strict mode 적용에 의한 변화

#### 일반 함수의 this

strict mode에서 함수를 일반 함수로서 호출하면 this에 undefined가 바인딩됨

- 생성자가 아닌 일반 함수 내부에서는 this를 사용할 필요가 없기 때문임

```javascript
(function () {
  "use strict",
    function foo() {
      console.log(this); //undefined
    };
  foo();

  function Foo() {
    console.log(this); //foo
  }
  new Foo();
})();
```

#### arguments 객체

- strict mode에서는 매개변수에 전달된 인수를 재할당해서 변경해도 arguments 객체에 반영되지 않음

```javascript
(function (a) {
  "use strict";
  //매개변수에 전달된 인수를 재할당하여 변경
  a = 2;

  //변경된 인수가 arguments 객체에 반영되지 않음
  console.log(arguments); //{0: 1, length: 1}
})(1);
```
