# JS 스터디 3일

> 『자바스크립트 완벽가이드』 를 읽고 핵심 내용과 추가로 개념들을 습득하며 정리하였습니다.

## 타입, 값, 변수

### 심볼(Symbol)

객체의 고유한 식별자(unique identifier)를 만들 때 사용. 자바스크립트는 객체 프로퍼티 키로 오직 문자형과 심볼형만을 허용.

```js
let id1 = Symbol("id")
let id2 = Symbol("id")
console.log(id1, id2) // Symbol(id) Symbol(id)
console.log(id1 == id2) // false
```

심볼은 유일성이 보장되는 자료형이기 때문에 절대 같은 값을 반환하지 않는다. 같은 인자로 호출하더라도 다른 값을 반환한다.

심볼인 프로퍼티 이름을 사용하고 그 심볼을 공유하지 않는다면 프로그램의 다른 모듈에서 실수로 프로퍼티를 덮어 쓸 일이 없다(라이브러리를 만들어야할 때 유용함). 외부에서 접근이 불가능하고 값을 덮어쓸 수 없음. (+) 키가 심볼인 프로퍼티는 `for..in` 반복문에서 배제.

```js
function getObject() {
    const world = Symbol()

    return {
        hello: 10,
        [world]: 10
    }
}

const a = getObject()
console.log(a) // {hello: 10, Symbol(): 10}
a.world = 20
console.log(a) // {hello: 10, world: 20, Symbol(): 10}
```

심볼은 ES6에서 for/of 루프와 이터러블 객체를 도입했을 때 클래스가 자기 자신을 이터러블로 만들 수 있는 표준 메서드를 정의해야 했다. 하지만 특정 문자열 이름을 이터레이터 메서드로 표중화하면 기존 코드가 깨지는 것을 피할 수 없었기에 심볼 이름을 도입한 것.

#### 전역 심볼

> 앞서 살펴본 것처럼, 심볼은 이름이 같더라도 모두 별개로 취급됩니다. 그런데 이름이 같은 심볼이 같은 개체를 가리키길 원하는 경우도 가끔 있습니다. 애플리케이션 곳곳에서 심볼 `"id"`를 이용해 특정 프로퍼티에 접근해야 한다고 가정해 봅시다.
> 
> *전역 심볼 레지스트리(global symbol registry)* 는 이런 경우를 위해 만들어졌습니다. 전역 심볼 레지스트리 안에 심볼을 만들고 해당 심볼에 접근하면, 이름이 같은 경우 항상 동일한 심볼을 반환해줍니다.
> 
> https://ko.javascript.info/symbol

레지스트리 안에 있는 심볼을 읽거나, 새로운 심볼을 생성하려면 `Symbol.for(key)` 사용

### 전역 객체

전역 객체의 프로퍼티는 전역으로 정의된 식별자이며 모든 자바스크립트 프로그램에서 사용할 수 있다. 자바스크립트 인터프리터를 시작할 때마다(또는 웹 브라우저가 새 페이지를 로드할 때마다) 다음과 같은 프로퍼티를 가진 새 전역 객체를 생성한다.

- undefined, Infinity, Nan 같은 전역 상수

- isNaN(), parseInt(), eval() 같은 전역 함수

- Date(), RegExp(), String(), Object(), Array() 같은 생성자 함수

- Math와 JSON 같은 전역 객체

브라우저 환경에선 전역 객체를 `window`, Node.js 환경에선 `global`라고 부르는데, 각 호스트 환경마다 부르는 이름은 다르다.

`globalThis` 속성은 환경에 무관하게 전역 `this` 값, 즉 전역 객체에 접근하는 표준 방법을 제공.

### 불변인 기본 값과 가변인 객체 참조

기본 값은 불변이므로 '변경'하는 방법은 없다. 기본 값은 또한 값으로 비교한다. 두 값이 일치하려면 값이 같아야 한다.

객체 값은 가변이므로 값을 바꿀 수 있다. 객체는 참조로 비교한다. 두 값이 일치하려면 같은 객체를 참조해야 한다.

```js
let user1 = {
  name: "Kim",
  age: 30
}
let user2 = user1
let user3 = {
  name: "Kim",
  age: 30
}

console.log(user1 == user2) // true
console.log(user1 == user3) // false
```

객체나 배열을 변수에 할당하는 것은 참조를 할당하는 것. 객체나 배열의 사본을 만들기 위해서는 반드시 객체 프로퍼티나 배열 요소를 직접 복사해야 한다.

별개의 객체나 배열을 비교할 떄는 양쪽의 프로퍼티나 요소를 비교.

### 타입 변환

#### 명시적 변환

개발자에 의해 의도적으로 값의 타입을 변환하는 것을 **명시적 타입 변환(Explicit coercion) 또는 타입 캐스팅(Type casting)**.

일치 연산자 `===`는 두 피연산자가 다른 타입이면 같지 않다고 판단.

#### 문자형으로 변환

```js
String(1) // "1"
String(NaN) // "NaN"
(1).toString() // "1"
(true).toString() // "true"
```

#### 숫자형으로 변환

`Number()` 는 문자열이나 다른 값을 숫자 유형으로 변환한다. 리터럴의 일부가 아닌 문자는 모두 무시.

`parseInt()`는 정수 부분만 유지, `parseFloat()`는 정수와 부동 소수점 숫자 모두 유지. 모두 시작 부분의 공백은 무시하고 숫자를 최대한 해석한 뒤 숫자가 아닌 문자가 등장하면 그 뒤는 모두 무시. 첫 번째 문자가 유효한 숫자 리터럴이 아니라면 모두 NaN 반환.

```js
Number('0') // 0
Number('10.1') // 10.1
Number('3m') // NaN
Number(true) // 1
parseInt('0') // 0
parseInt('3 4 m') // 3
parseFloat('3.14') // 3.14
parseFloat('$3.14') // NaN
```

#### 불린형으로 변환

```js
Boolean(0) // false
Boolean(' 123') // true
Boolean(undefined) // false
```

#### 암묵적 타입 변환

개발자의 의도와는 상관없이 자바스크립트 엔진에 의해 암묵적으로 타입이 자동 변환하는 것을 **암묵적 타입 변환(Implicit coercion) 또는 타입 강제 변환(Type coercion)**.

자바스크립트는 값의 타입을 강제하지 않는다. 자바스크립트가 문자열을 예상한다면 그 자리에 있는 값은 무엇이든 문자열로, 숫자를 예상한다면 그 자리에 있는 값을 숫자로 변환하려 시도한다.

숫자로 인식할 수 있는 문자열은 그 숫자로 변환. 숫자 리터럴이 아닌 문자가 앞뒤에 있다면 NaN으로 변환.

암묵적 타입 변환이 발생하면 문자열, 숫자, 불리언과 같은 원시 타입 중 하나로 타입을 자동 변환한다.

##### 문자형으로 변환

문자형으로의 변환은 대부분 예측 가능한 방식으로 일어난다.

```js
1 + '2' // "12"
NaN + '' // "NaN"
Infinity + '' // "Infinity"
true + '' // "true"
null + '' // "null"
undefined + '' // "undefined"
({}) + '' // "[object object]"
[] + '' // ""
[10, 20] + '' // "10,20"
```

##### 숫자형으로 변환

수학과 관련된 함수와 표현식에서 자동으로 일어난다. 피연산자를 숫자 타입으로 변환할 수 없는 경우는 산술 연산을 수행할 수 없으므로 NaN을 반환한다. 숫자 이외의 글자가 들어가 있는 문자열을 숫자형으로 변환하려고 하면 NaN.

```js
1 - '1'    // 0
1 * '10'   // 10
1 / 'one'  // NaN
```

비교 연산자는 피연산자의 크기를 비교하므로 피연산자는 숫자 타입이어야 한다. 

```js
1 > '2' // false
-3 < null // true
```

숫자 타입 아닌 값을 숫자 타입으로 암묵적 타입 변환을 수행할 때,  + 단항 연산자는 피연산자가 숫자 타입의 값이 아니면 숫자 타입의 값으로 암묵적 타입 변환을 수행.

```js
+'' // 0
+'5' // 5
-'3' // -3
+true // 1
+false // 0
+'one' // NaN
+{} // NaN
+[] // 0
+[1] // 1
+[1, 2] // NaN
```

#### 불린형으로 변환

false로 평가되는 값

- false

- undefined

- null

- 0, -0

- NaN

- "" (빈 문자열)

논리 연산을 수행할 때 발생. 0, 빈 문자열, null, undefined, NaN과 같이 비어있다고 느껴지는 값은 `false`, 그 외의 값은 `true`.

⚠ 문자열 `"0"`은 `true`

```js
!true // false
!undefined // true
!!0 // false
```

#### 객체를 기본 값으로 변환

자바스크립트 일부 객체는 여러 가지 기본 값으로 표현될 수 있다. 객체 형 변환은 세 종류로 구분되는데, 'hint’라 불리는 값이 구분 기준이 된다.

- "string"  -  문자열을 기대하는 연산을 수행할 때

- "number" - 수학 연산을 적용하려 할 때

- "default" - 연산자가 기대하는 자료형이 확실치 않을 때

객체는 논리 평가 시 단 하나의 예외 없이 `true`를 반환

### 

## 추가 학습 예정

- globalThis
- 객체 원시형으로 변환 (3.9.3)
  - https://ko.javascript.info/object-toprimitive

## Reference

- [심볼형](https://ko.javascript.info/symbol)
- [자바스크립트 Symbol, 어디에 쓸까요? - YouTube](https://www.youtube.com/watch?v=11HkEyCrriE)
- [참조에 의한 객체 복사](https://ko.javascript.info/object-copy)
- [Type coercion | PoiemaWeb](https://poiemaweb.com/js-type-coercion)
- 
