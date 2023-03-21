# JS 스터디 2일

> 『자바스크립트 완벽가이드』 를 읽고 핵심 내용과 추가로 개념들을 습득하며 정리하였습니다.

## 타입, 값, 변수

자바스크립트 타입은 기본 타입과 객체 타입으로 나뉜다.

- 기본타입: 숫자, 문자열, 불, null, undefined, 심볼

- 숫자, 문자열, 불, 심벌, null, undefined 중 어느 것에도 속하지 않는 값은 모두 객체

자바스크립트 객체는 이름 붙은 값의 순서 없는 집합. 배열은 자바스크립트의 특별한 객체로 값의 순서 있는 집합이며 각 값은 숫자로 표현된다.



자바스크립트의 함수와 클래스는 그저 문법의 일부분이 아닌 그 자체가 값이므로 프로그램에서 조작할 수 있다.

자바스크립트 인터프리터는 자동으로 가비지 컬렉션을 수행해 메모리를 관리.

자바스크립트는 객체 지향 프로그래밍 스타일을 지원. 함수가 다양한 타입의 값을 다루는 것이 아니라 객체(타입 자체)에 그 값을 다루는 메서드를 정의. 예) 배열 a의 요소 정렬 `a.sort()`

자바스크립트의 객체 타입은 가변이며 기본 타입은 불변.

자바스크립트는 값의 타입을 자유롭게 변환

### 숫자

자바스크립트의 숫자 타입인 `Number`는 정수와 함께 실수를 대략적으로 표현. IEEE 754 표준에서 정의하는 64비트 부동 소수점 형식을 사용. [JavaScript Numbers](https://www.javascripttutorial.net/javascript-number/)

#### 64비트 부동 소수점

```js
console.log(0.1 + 0.2); // 0.30000000000000004
```

컴퓨터는 이진법을 사용해서 데이터를 표현하고 처리한다.

1/3을 소수로 나타내면 0.3333333... 무한소수. 이진법으로 숫자를 표현하기 때문에 표현할 수 있는 숫자의 개수는 비트 수에 비례한다. 따라서, 무한개인 모든 수(실수)를 표현할 수 없으므로 몇 개의 자리까지만 표현을 한다. 그 이유로 생략된 숫자만큼의 오차가 발생한다.

> IEEE754에 정의된 스펙에는 실수형을 표현하는 방법에는 2가지가 있다. *single precision* 과 *double precision* 이 그것인데, 두 방법은 비트 수만 제외하고 전부 동일하기 때문에 JS에서 사용하는 *double precision* 에 대해서만 살펴보면 된다. [JS에서의 64비트 부동소숫점](https://medium.com/@rnrjsah789/js%EC%97%90%EC%84%9C%EC%9D%98-64%EB%B9%84%ED%8A%B8-%EB%B6%80%EB%8F%99%EC%86%8C%EC%88%AB%EC%A0%90-c95e0cfec2b2)

![](https://miro.medium.com/v2/resize:fit:810/1*3BQhsOh0bXtZ8Mg3bnuOwg.png)

*double precision*은 64비트를 가지고 실수를 표현하는 방법이다. *sign*(부호) 가 1bit / *exponent*(지수)가 11bit / *fraction*(가수)이 52bit 를 사용한다.

- sign는 +1 또는 -1  
- exponential은 2¹¹-1 = 2047  
- fraction은 2⁵² -1

![](https://miro.medium.com/v2/resize:fit:563/1*EZGy650SWjXIxYghgfPz3Q.png)

bias는 -1024 ~ +1024를 만들어 주기 위해

```
1. 1592.125(10)을 이진수로 표현하면, 11000111000.001(2)이다.
2. double precision으로 표현하기 위해 (1.fraction) *2^n형태로 정규화한다.
3. 위에 따라 소숫점을 뒤로 옮겨 주면 1.1000111000001(2) * 2¹⁰ 으로 나타낼수 있다.
4. 이를 비트로 표현하면 아래와 같이 된다. (지수부에 bias 더한다)
1 / 100 0000 1010 / 0… 11000111000001
(sign / exponential / fraction)
```

##### 바이어스 표현법

> 지수부에는 따로 부호 비트가 없기 때문에 음수 지수를 처리하기 위해 보통 바이어스 표현법을 사용한다. 즉, 할당된 자릿수로 표현 가능한 전체 영역을 반으로 나누어, 작은 영역에는 음수값 및 0, 큰 영역에는 양수값이 차례대로 1:1 대응되도록 한다. 예를 들어, 지수부를 8비트로 표현한다면 모두 256가지 수를 나타낼 수 있는데 이것을 반으로 나누어 음수 127개와 0, 양수 128개를 차례대로 대응시킨다. 
> 
> 따라서, 비트열 00000000은 지수 -127을 나타내고, 01111111은 지수 0, 11111111은 지수 128을 나타낸다. 일반적으로는 지수부가 n비트일 때 (2n / 2 - 1 = 2n-1 - 1)을 지수 값에 더하며 이것을 바이어스 상수라고 한다. [부동소수점 - 위키백과, 우리 모두의 백과사전](https://ko.wikipedia.org/wiki/%EB%B6%80%EB%8F%99%EC%86%8C%EC%88%98%EC%A0%90)

이에 따라 최대  ±1.7976931348623157e+308, 최소 ±5e-324 범위의 숫자를 표현할 수 있다.

```js
console.log( Number.MAX_VALUE );
// 1.7976931348623157e+308

console.log( Number.MIN_VALUE );
// 5e-324
```

> `MAX_SAFE_INTEGER` 상수는 `9007199254740991`(9,007,199,254,740,991 또는 약 9000조)의 값을 갖고 있습니다. 이 값의 이유는 JavaScript가 [IEEE 754](http://en.wikipedia.org/wiki/IEEE_floating_point)에 기술된 [배정밀도 부동소숫점 형식 숫자체계](http://en.wikipedia.org/wiki/Double_precision_floating-point_format)를 사용하기 때문으로, 이로 인해 `-(2^53 - 1)`과 `2^53 - 1` 사이의 수만 안전하게 표현할 수 있습니다.
> 
> 여기서의 안전함이란 정수를 정확하고 올바르게 비교할 수 있음을 의미합니다. 예를 들어 `Number.MAX_SAFE_INTEGER + 1 === Number.MAX_SAFE_INTEGER + 2`는 참으로 평가되며 이는 수학적으로 올바르지 않습니다. 더 자세한 내용은 [`Number.isSafeInteger()`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Number/isSafeInteger)를 참고하세요.
> 
> [Number.MAX_SAFE_INTEGER - JavaScript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Number/MAX_SAFE_INTEGER)

하지만 배열 인덱싱이나 비트 연산자 등 일부 자바스크립트 연산은 32비트 정수를 사용.

자바스크립트 프로그램에 직접 기입한 숫자를 숫자 리터럴이라 부른다.

#### 정수 리터럴

10진 정수는 연속된 숫자로 표현. 10진 정수 리터럴뿐 아니라 16진수, 8진수, 2진수로 정수를 표현할 수도 있다.

#### 부동 소수점 리터럴

실수의 전통적 문법 사용. 값은 정수 부분, 소수점, 소수점 아래 부분을 순서대로. 부동 소수점 리터럴은 지수 표기법으로도 표현 가능.

예) `3.14, .33333, 6.02e23, 1.4524E-32` 

(+) 리터럴 안에 밑줄을 써서 리터럴을 읽기 쉽게 나눠 쓸 수 있다. `1_000_000`

#### 자바스크립트 산술 연산

[표현식과 연산자 - JavaScript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Expressions_and_Operators#%EC%82%B0%EC%88%A0_%EC%97%B0%EC%82%B0%EC%9E%90)

기본 산술 연산자 외에도 자바스크립트 Math 객체의 프로퍼티로 정의된 함수와 상수를 통해 더 복잡한 수학 계산 지원. [Math - JavaScript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Math)

자바스크립트는 산술 연산 과정에 0으로 나누거나 오버플로, 언더플로가 발생해도 에러 발생X. 오버플로에는 Infinity 반환, 언더플로에는 -Infinity 반환. 0을 0으로 나누는 경우, 무한대%무한대, 음수의 제곱근을 구하려하는 경우, 숫자가 아니고 숫자로 변환할 수 없는 피연산자에 산술 연산자를 적용하려 할 때는 NaN(숫자가 아님)을 반환.

NaN 값은 자기 자신을 포함해 어떤 값과도 같지 않음. NaN인지 알아보려면 `Number.isNaN(x)` 사용.

```js
0 === -0 // true
0 / 0 // NaN
Infinity / 3 // Infinity
3 / Infinity // 0
3 / NaN // NaN
Infinity === -Infinity // false
```

#### BigInt로 임의 정확도를 부여한 정수

> **`BigInt`** 는 [`Number`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Number) 원시 값이 안정적으로 나타낼 수 있는 최대치인 2^53 - 1보다 큰 정수를 표현할 수 있는 내장 객체입니다. [BigInt - JavaScript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/BigInt)

`BigInt`는 정수 리터럴의 뒤에 `n`을 붙이거나 함수 `BigInt()`를 호출해 생성할 수 있다. BigInt 값의 산술 연산 중 나눗셈을 할 때는 나머지를 버린다. 

BigInt 피연산자와 일반적인 숫자 피연산자를 섞어 쓸 수는 없다. 

```js
1000n + 2000n // 3000n
3000n % 997n // 9n
```

#### 날짜와 시간

Date 클래스는 날짜와 시간에 대응하는 숫자를 표현하고 조작.

### 텍스트

자바스크립트에서 텍스트를 표현하는 타입은 문자열. 문자열은 16비트 값이 순서에 따라 이어진 형태이며, 기본 값이므로 불변.

각 값은 일반적으로 유니코드 문자이며 문자열의 길이는 그 문자열에 포함된 16비트 값의 개수. (자바스크립트는 유니코드 문자셋의 UTF-16 인코딩 사용)

#### 문자열 리터럴

문자열을 사용할 떄는 작은 따옴표('), 큰따옴표("), 백틱(`)을 쌍으로 묶는다.

(+) 문자열 리터럴 안의 이스케이프 시퀀스 [String - JavaScript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String)

```js
""
"3.14"
"He's a doctor."
'name'
`'one' "two"`
```

#### 문자열 다루기

`+` 연산자를 문자열에 쓰면 두 번째 문자열을 첫 번째 문자열 뒤에 이어 붙인다.

```js
"Helo, " + "world!" // "Hello, world!"
```

문자열을 비교할 때는 `===`, `!==` 연산자.

#### 템플릿 리터럴

ES6부터는 백틱으로 감싼 문자열 리터럴 사용 가능. 일반적인 문자열 리터럴 문법과는 다르게, 임의의 자바스크립트 표현식을 넣을 수 있는 템플릿 리터럴.  `${ }` 안에 있는 것은 모두 자바스크립트 표현식을 해석. 

#### 태그된 템플릿 리터럴

백틱 바로 앞에 함수 이름(태그)이 있으면 템플릿 리터럴의 텍스트와 표현식 값이 함수에 전달. '태그된 템플릿 리터럴'의 값이 함수의 반환 값.

![image](https://user-images.githubusercontent.com/82589401/205785152-3c6bee5d-958b-48c2-bd57-5c9c97a75c9b.png)

백틱 문자가 여닫는 괄호를 대신.

### 불 값

불 값은 참 또는 거짓을 표현. `true`, `false`

자바스크립트 값은 모두 불 값으로 변환될 수 있다.

**자바스크립트에서 false로 간주되는 것들 (아래)**

```js
undefined, null
NaN
0, -0
""
false
```

### null과 undefined

`null`은 값이 없음을 나타낼 때 사용하는 특별한 값. 

`undefined`는 초기화되지 않은 변수의 값. (+) 존재하지 않는 객체 프로퍼티나 배열 요소에 접근했을 때 반환되는 값.

```js
console.log(typeof null) // object
console.log(typeof undefined) // undefined
console.log(null == undefined) // true
```



## 추가 학습 예정

- [BigInt - JavaScript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/BigInt)

- 정규표현식

- [JS의 객체는 hash table이 아닙니다!](https://velog.io/@wongue_shin/JS%EC%9D%98-%EA%B0%9D%EC%B2%B4%EB%8A%94-hash-table%EC%9D%B4-%EC%95%84%EB%8B%99%EB%8B%88%EB%8B%A4)

## Reference

- [Array | PoiemaWeb](https://poiemaweb.com/js-array-is-not-arrray)
