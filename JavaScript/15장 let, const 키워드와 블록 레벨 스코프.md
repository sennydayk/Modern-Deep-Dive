# 15장 let, const 키워드와 블록 레벨 스코프

## var 키워드로 선언한 변수의 문제점

### 변수 중복 선언 허용

`var` 키워드로 선언한 변수는 중복 선언이 가능하다.

```javascript
var x = 1;
var y = 1;

// var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용한다.
// 초기화문이 있는 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작한다.
var x = 100;
var y; // 초기화문이 없는 변수 선언문은 무시된다.

console.log(x); // 100
console.log(y); // 1
```

### 함수 레벨 스코프

`var` 키워드로 선언한 변수는 오로지 함수의 코드 블록만을 지역 스코프로 인정한다. 따라서 함수 외부에서 `var` 키워드로 선언한 변수는 코드 블록 내에서 선언해도 모두 전역 변수가 된다.

```javascript
var x = 1;

if (true) {
  var x = 10; //코드 블록 내에서 선언해도 모두 전역 변수가 된다.
}

console.log(x); // 10

var i = 10;

// for문에서 선언한 i는 전역 변수다.
for(var i = 0; i < 5; i++) {
  console.log(i); // 0 1 2 3 4
}

// 의도치 않게 i의 값이 변경되었다.
console.log(i); // 5
```

### 변수 호이스팅

`var` 키워드로 변수를 선언하면 변수 호이스팅에 의해 변수 선언문이 스코프의 선두로 끌어 올려진 것처럼 동작한다. 즉, 변수 호이스팅에 의해 `var` 키워드로 선언한 변수는 변수 선언문 이전에 참조할 수 있다. 단, 할당문 이전에 변수를 참조하면 언제나 `undefined`를 반환한다.

```javascript
// 이미 foo라는 변수가 선언되었다.
// 변수 선언문 이전에 참조가 가능하다.
console.log(foo); // undefined 초기화 단계

foo = 123; // 할당 단계

console.log(foo); // 123

var foo; // 변수 선언은 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 실행된다.
```

이러한 플로우는 에러를 발생시키지는 않지만 프로그램의 흐름상 맞지 않을 뿐더러 가독성을 떨어뜨리고 오류를 발생시킬 여지를 남긴다.

## let 키워드

### 변수 중복 선언 금지

```javascript
let bar = 123;

let bar = 456; // SyntaxError 
// 중복 선언을 허용하지 않는다.
```

### 블록 레벨 스코프

`let` 키워드로 선언한 변수는 모든 코드 블록(함수, if문, for문, while문 등)을 지역 스코프로 인정하는 블록 레벨 스코프를 따른다.

```javascript
let foo = 1; // 전역 변수

{
  let foo = 2; // 지역 변수(전역 변수와 다른 별개의 변수)
  let bar = 3; // 지역 변수
}

console.log(foo); // 1
console.log(bar); // ReferenceError: bar is not defined

let i = 10; // 전역 스코프

function foo() {
  let i = 100;
  for(let i = 1; i < 3; i++) {
    console.log(i); // 1 2  블록 레벨 스코프
  }
  console.log(i); // 100 함수 레벨 스코프
}

foo();
console.log(i); // 10
```

### 변수 호이스팅

`let` 키워드로 선언한 변수는 "선언 단계"와 "초기화 단계"가 분리되어 진행된다. 즉, 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 선언 단계가 먼저 실행되지만 초기화 단계는 변수 선언문에 도달했을 때 실행된다.

```javascript
//초기화 단계 전인 선언 단계에서는 변수를 참조할 수 없다.
// 일시적 사각지대(TDZ Temporal Dead Zone)
console.log(foo); // ReferenceError: foo is not defined 선언 단계

let foo; // 초기화 단계
console.log(foo); // undefined

foo = 1; // 할당 단계
console.log(foo); // 1
```

![image](https://github.com/user-attachments/assets/124de077-a036-4c86-a253-48bc65f12580)

만약 초기화가 실행되기 이전에 변수에 접근하려고 하면 참조 에러가 발생한다. 스코프의 시작 지점부터 초기화 시작 지점까지 변수를 참조할 수 없는 구간을 **일시적 사각지대**(TDZ)라고 부른다.

이러한 현상으로 인해 `let` 키워드는 변수 호이스팅이 발생하지 않는 것처럼 보이지만, 사실은 **호이스팅이 발생하기 때문에 참조 에러(ReferenceError)가 발생하는 것**이다.

### 전역 객체와 let

`var` 키워드로 선언한 전역 변수와 전역 함수, 선언하지 않은 변수에 값을 할당한 암묵적 전역은 전역 객체 window의 프로퍼티가 된다.

```javascript
// 브라우저 환경에서 실행

var x = 1; // 전역 변수
y = 2; // 암묵적 전역
function foo() {}; // 전역 함수

console.log(window.x); // 1 
console.log(x); // window 생략 가능

console.log(window.y); // 2
console.log(foo); // f foo() {}

let x = 1;
console.log(window.x); // undefined
console.log(x); // 1
```

`let`과 `const` 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다. 즉 위 예제의 window.x 와 같이 접근할 수 없다.

## const 키워드

### 선언과 초기화

`const` 키워드로 선언한 변수는 반드시 선언과 동시에 초기화해야 한다.

`let` 키워드로 선언한 변수와 마찬가지로 블록 레벨 스코프를 가지며, 변수 호이스팅이 발생하지 않는 것처럼 동작한다.

```javascript
const foo = 1;
// 선언만 할 경우 문법 에러가 발생한다.
const x; // SyntaxError
```

### 재할당 금지

`var` 또는 `let` 키워드로 선언한 변수는 재할당이 자유로우나 `const` 키워드로 선언한 변수는 재할당이 금지된다.

```javascript
const foo = 1;
foo = 2; // TypeError
```

### 상수

변수의 상대 개념인 상수는 재할당이 금지된 변수를 말한다. const 키워드로 선언한 변수에 원시 값을 할당한 경우 변수 값을 변경할 수 없다.

상수는 상태 유지와 가독성, 유지보수의 편의를 위해 적극적으로 사용해야 한다.

### const 키워드와 객체

`const` 키워드로 선언된 변수에 객체를 할당한 경우 값을 변경할 수 있다. 객체는 재할당 없이도 직접 변경이 가능하기 때문이다.

```javascript
const person = {
  name: 'lee'
};

person.name = 'kim'; // 재할당 없이 변경이 가능하다.

console.log(person); // {name: 'kim'}
```

즉, `const` 키워드는 재할당을 금지할 뿐 불변을 의미하지는 않는다.

> 💡 _const 키워드는 재할당을 금지하지만 불변을 의미하지 않는다는 표현에 의문이 생겼다. 구체적으로 어떤 의미일까?_

아래 예제를 보자.

```javascript
const person = {
  Name: "Lee",
  Age: 25
};

person.Name = "Kim";  // 1번
person = { Name : "Kim" };  // 2번
```

콘솔에 출력해 보면 1번과 2번은 각각 어떤 결과값이 나올지 생각해보자.

정답은 1번은 변경이 가능하므로 {Name: "Kim", Age: 25} 로 속성이 변경된 `person` 객체가 출력된다.

반면 2번은 에러가 발생한다. 1번과 똑같이 객체 내의 속성을 변경하는 로직처럼 보이지만 2번은 `person` 객체를 재할당하는 개념이다. 그러므로 `person` 객체의 새로운 메모리 주소가 생성된다.

![image](https://github.com/user-attachments/assets/812aa5eb-5c41-4057-96e3-0c880be27d0e)


11장 원시 값과 객체의 비교 부분에 나왔던 그림이다.

자바스크립트에서 객체를 정의하면 해당 객체의 메모리 주소 값을 참조한다.

이때 `const` 키워드는 객체의 메모리 주소 값이 변경되지 않도록 보장한다. 즉 그림에서 보면 0x00001332 라는 참조 값이 변경되지 않도록 보장하는 것이고, 따라서 해당 주소를 참조하는 객체 내의 속성인 `name`은 변경할 수 있는 것이다.

정리하면 다음과 같다.

- `const` 키워드는 객체에 대한 참조값이 일정하게 유지되도록 보장한다.
- 다만 JavaScript의 객체는 변경 가능하므로 객체의 속성은 변경이 가능하다.
- 따라서 객체가 `const`로 선언된 경우 새 객체에 재할당할 수 없으며, 참조하는 객체의 속성은 수정이 가능하다.

## var vs. let vs. const

변수 선언에는 기본적으로 `const`를 사용하고 `let`은 재할당이 필요한 경우에 한정해 사용하는 것이 좋다. `const` 키워드를 사용하면 의도치 않은 재할당을 방지하기 때문에 좀 더 안전하다. 재할당이 반드시 필요해진다면 `let` 키워드로 변경해 사용한다.
