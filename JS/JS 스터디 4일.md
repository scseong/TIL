# JS 스터디 4일

> 『자바스크립트 완벽가이드』 를 읽고 핵심 내용과 추가로 개념들을 습득하며 정리하였습니다.

## 변수 선언과 할당

변수는 데이터를 저장할 때 쓰이는 ‘이름이 붙은 저장소’. 변수 선언은 값을 저장하기 위한 메모리 공간을 확보하고 변수 이름과 확보된 메모리 공간의 주소를 연결해서 값을 저장할 수 있게 준비하는 것. 준비된 변수에 실제 값을 할당.

### 자바스크립트에서 변수 선언 방법

#### 자바스크립트의 변수 선언 방법

1. `var, let, const` 키워드를 사용해 변수를 선언
   
   - [`var`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/var)
     
     - 변수를 선언. 추가로 동시에 값을 초기화.
   
   - [`let`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/let)
     
     - 블록 스코프 지역 변수를 선언. 추가로 동시에 값을 초기화.
   
   - [`const`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/const)
     
     - 블록 스코프 읽기 전용 상수를 선언.

2. [구조 분해 할당](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) 구문을 사용하여 [객체 리터럴](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Grammar_and_types#%EA%B0%9D%EC%B2%B4_%EB%A6%AC%ED%84%B0%EB%9F%B4)에서 값을 풀기 위해 변수를 선언

3. 간단히 변수에 값을 할당 `x = 1` (선언 되지 않은 전역변수). 사용하지 말 것.

#### 자바스크립트 엔진의 변수 선언

- 선언 단계: 변수 이름을 등록해서 자바스크립트 엔진에 변수의 존재를 알린다

- 초기화 단계: 값을 저장하기 위한 메모리 공간을 확보하고 암묵적으로 undefined를 할당해 초기화한다
  
  > 초기화(initialization): 변수가 선언된 이후 최초로 값을 할당하는 것

### var, let, const 차이

`var`는 동일한 변수명으로 재선언하여도 에러가 발생하지 않는다. 또한, 함수 바디 바깥에서 `var`를 사용하면 전역 변수로 선언된다. 

`const, let`은 블록 레벨 스코프.`let` 은 재할당은 가능하지만 재선언은 불가능. `const`는 재선언, 재할당 모두 불가능(상수)

> 상수: 다시 선언되거나 할당될 수 없는 변수. 값에 이름을 영구히 할당.

#### const를 써야할 때

**const 키워드 사용에 대한 두 가지 의견**

1. 기본적으로 바뀌지 않는 값에만 사용

2. 모든 것을 const로 선언한 다음, 실제로 값을 바꿔야 한다고 인식했을 때 let 선언으로 변경. ➡ 의도치 않게 변수 값을 바꾸는 실수를 방지

(+) for/in, for/of 루프의 변수 선언할 때

루프 내부에서 새 값을 할당하지 않는다면 `const`로 변수 선언할 수 있다. 이 경우 `cosnt` 선언은 루프의 반복 주기 안에서는 값이 바뀌지 않는다는 의미를 가짐.

```js
for (const item of arr) console.log(item)
```

### 호이스팅

**변수 호이스팅(hoisting)**: 변수 선언문이 코드의 선두로 끌어 올려진 것처럼 동작하는 자바스크립트 고유의 특징.

```javascript
console.log(score); // undefined

var score; // 변수 선언문
```

변수 선언이 소스코드가 한 줄씩 순차적으로 실행되는 시점, 즉 런타임(runtime)이 아니라 그 이전 단계에서 먼저 실행

자바스크립트 엔진은 변수 선언이 소스코드의 어디에 있든 상관없이 다른 코드보다 먼저 실행➡변수 선언이 소스코드의 어디에 위치하는지와 상관없이 어디서든지 변수를 참조 가능

⚠const, let 역시 호이스팅을 지원한다. [참고](https://medium.com/korbit-engineering/let%EA%B3%BC-const%EB%8A%94-%ED%98%B8%EC%9D%B4%EC%8A%A4%ED%8C%85-%EB%90%A0%EA%B9%8C-72fcf2fac365)

## 추가 학습 예정

- 구조분해할당

- 스코프(scope)

- TDZ

## Reference

- [변수와 상수](https://ko.javascript.info/variables#ref-220)

- [문법과 자료형 - JavaScript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Grammar_and_types)

- [Var, Let, and Const – What's the Difference?](https://www.freecodecamp.org/news/var-let-and-const-whats-the-difference/)
