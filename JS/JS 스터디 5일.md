# JS 스터디 5일

> 『자바스크립트 완벽가이드』 를 읽고 핵심 내용과 추가로 개념들을 습득하며 정리하였습니다.

## 표현식과 연산자

표현식(expression)이란 어떤 값으로 평가되는 문. 표현식이 평가되면 새로운 값을 생성하거나 기존의 값을 참조한다. **값으로 평가될 수 있는 문은 모두 표현식**

문(statement)은 프로그램을 구성하는 기본 단위이자 최소 실행 단위.

상수와 같은 단순한 표현식부터 단순한 표현식을 조합해 복잡한 표현식을 만들 때는 대부분 연산자를 사용.

연산자(operator)는 하나 이상의 표현식을 대상으로 연산을 수행해 하나의 값을 만든다. 연산자는 피연산자의 값을 어떤 형태로 조합해 새 값으로 평가한다.

### 기본 표현식

가장 단순한 표현식.

- 리터럴 값

- 일부 키워드 - true, false, null, this

- 변수 참조

#### 객체와 배열 초기화 표현식

객체와 배열의 초기화 표현식은 그 값이 새로 생성된 객체나 배열인 표현식. 객체 리터럴이나 배열 리터럴이라고 부르기도 하지만 기본 표현식은 아니다.

배열 초기화 표현식은 `[], [1, 2]` 대괄호 안에 콤마로 구분된 리스트를 쓰는 형태. 콤마 사이의 값을 생략하면 정의되지 않은 요소가 그 자리에 들어가고 마지막 표현식 다음의 인덱스로 배열에 접근하는 표현식은 undefined로 평가.

객체 초기화 표현식은 `let coord = { x: 1, y: 2};`  중괄호를 쓰고 각 하위 표현식은 프로퍼티 이름과 콜론으로 시작.

### 함수 정의 표현식

함수 정의 표현식은 함수를 정의하며 그 값은 함수. 함수 리터럴이라고도 부를 수 있다. `function` 키워드로 시작하고 괄호 안에 0개 이상의 식별자를 쓴 다음, 중괄호 안에 코드를 작성.

`const add = function(x, y) { return x + y; };`

### 프로퍼티 접근 표현식

객체 프로퍼티나 배열 요소의 값으로 평가. 자바스크립트의 두 가지 프로퍼티 접근 문법

- `expression.identifier`
  
  - 표현식 뒤에 마침표를 쓰고 그 뒤에 식별자. 표현식은 객체, 식별자는 프로퍼티 이름

- `expression[expression]`
  
  - 표현식 뒤에 대괄호를 쓰고 그 안에 다른 표현식. 두 번째 표현식은 프로퍼티 이름이나 배열 요소 인덱스. `.`, `[` 앞에 있는 표현식을 첫 번째로 평가.
  
  - `.` 문법은 접근하고자 하는 이름이 유효한 식별자이고 그 이름을 알고 있을 때만 사용 가능. 프로퍼티 이름에 공백, 구두점이 들어 있거나 숫자, 계산 결과인 경우 `[]` 표기법 사용.

#### 조건부 프로퍼티 접근

자바스크립트에서 프로퍼티를 가질 수 없는 값은 `null`, `undefined`뿐. 두 값에 프로퍼티 접근 표현식을 사용하면 TypeError 발생. `?.`, `?.[]` 문법을 사용해 에러 방지.

##### 옵셔널 체이닝(Optional chaining) 연산자

> **optional chaining** 연산자 (**`?.`**) 는 체인의 각 참조가 유효한지 명시적으로 검증하지 않고, 연결된 객체 체인 내에 깊숙이 위치한 속성 값을 읽을 수 있다.
> [Optional chaining - JavaScript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Optional_chaining)

좌항의 피연산자가 nullish(null 또는 undefined)인 경우 undefined를 반환하고, 그렇지 않으면 우항의 프로퍼티 참조. 옵셔널 체이닝 연산자가 도입되기 전에는 논리 연산자 `&&`를 사용한 단축 평가를 통해 확인했다. 논리 연산자 `&&`은 좌항 피연산자가 Falsy 값이면 좌항 피연산자를 그대로 반환. 

`?.`을 사용한 프로퍼티 접근이 <u>단축 평가</u>이기 때문에 좌항 피연산자가 null이나 undefined로 평가되면 더는 프로퍼티에 접근하려 시도하지 않고 전체 표현식을 즉시 undefined으로 평가.

> - 단축 평가: 표현식을 평가하는 도중에 평가 결과가 확정된 경우 나머지 평가 과정을 생략하는 것
> 
> - Falsy 값: false로 평가되는 값. 0, false, null, undefined, '', NaN

```js
const obj1 = { a: null };
console.log(obj1.a.b) // TypeError
console.log(obj1.a?.b) // undefined
```

##### (+) 널 병합(nullish coalescing) 연산자

> **널 병합 연산자 (`??`)** 는 왼쪽 피연산자가 [null](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/null) 또는 [undefined](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/undefined)일 때 오른쪽 피연산자를 반환하고, 그렇지 않으면 왼쪽 피연산자를 반환하는 논리 연산자이다.
> [Nullish coalescing operator - JavaScript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing)

널 병합 연산자는 좌항의 피연산자가 Falsy 값이라도 null 또는 undefined가 아니면 좌항의 피연산자를 그대로 반환. 변수에 기본값을 설정할 때 유용. 널 병합 연산자가 도입되기 이전에는 논리 연산자 `||`를 사용한 단축 평가를 통해 변수에 기본값 할당.

```js
const a = '' ?? 'hello';
console.log(a); // ""
```

### 호출 표현식

함수나 메서드를 호출(실행)하는 문법. 호출할 함수로 평가되는 함수 표현식으로 시작해 여는 괄호를 쓰고, 콤마로 구분된 0개 이상의 함수 인자 표현식 리스트를 쓰고 닫는 괄호로 끝난다.

`Math.max(x, y, z)` Math.max는 함수 x, y, z는 인자

호출 표현식 평가: 함수 표현식 평가하고 함수 인자 표현식 평가해 인자 값 리스트 생성 (함수 표현식의 값이 함수가 아니라면 TypeError) `const foo = ''; foo(); // TypeError` ➡인자 값을 함수를 정의할 때 지정된 함수 매개변수(parameter)에 순서대로 할당한 다음, 함수 바디를 실행 ➡ return 문을 사용해 값을 반환하면 그 값이 호출 표현식의 값. 그렇지 않다면 undefined 반환.

#### 조건부 호출

왼쪽에 있는 표현식이 null이나 undefined, 기타 함수가 아닌 것으로 평가될 때는 TypeError. `?.()` 호출 문법을 사용하면 왼쪽에 있는 표현식이 null이나 undefined로 평가될 때 호출 표현식 전체를 undefined로 평가.

```js
function square(x, log) {
    log?.(x);
    return x * x;
}
```

## 추가 학습 예정

- 
- 단락 평가
- 옵셔널 체이닝, 널 병합 연산자 사용

## Reference

- [표현식과 연산자 - JavaScript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Expressions_and_Operators)

- [옵셔널 체이닝 '?.'](https://ko.javascript.info/optional-chaining)

- [Function | PoiemaWeb](https://poiemaweb.com/js-function)
