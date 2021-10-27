## 함수 선언식 vs 함수 표현식

### 함수 선언식

```jsx
// 함수 선언식
function funcDeclarations() {
  return "A function declaration";
}
funcDeclarations(); // 'A function declaration'
```

- 일반 프로그래밍 언어에서 자주 쓰이는 방식

### 함수 표현식

```jsx
// 함수 표현식
const funcExpression = function () {
  return "A function expression";
};
funcExpression(); // 'A function expression'
```

### 차이점

- 함수 선언식은 호이스팅에 영향을 **받는다.**
- 함수 표현식은 호이스팅에 영향을 **받지 않는다.**

### 호이스팅

- 브라우저가 자바스크립트를 해석할 때 코드를 구현한 위치와 관계없이 **맨 위**로 끌어 올리는 것

```jsx
// 실행 전
logMessage();
sumNumbers();

function logMessage() {
  return "worked";
}

var sumNumbers = function () {
  return 10 + 20;
};
```

```jsx
// 실행 시
function logMessage() {
  return "worked";
}

var sumNumbers;

logMessage(); // 'worked'
sumNumbers(); // Uncaught TypeError: sumNumbers is not a function

sumNumbers = function () {
  return 10 + 20;
};
```

### 그럼 함수표현식은 왜 써?

- 호이스팅의 영향을 받지 않음
- **클로져**로 사용
  - 함수를 실행하기 전에 해당 함수에 변수를 넘기고 싶을 때 사용된다.
- **콜백**으로 사용 (다른 함수의 인자로 넘길 수 있음)

### 결론

- 두 차이점을 인지한 상태로 컨벤션을 잘 지키자
- 함수 선언식보다는 함수 표현식이 선호되는 경향이 있다.

## 클로저

- 내부함수가 외부함수의 맥락(context)에 접근할 수 있는 것

```jsx
function outerFunc() {
  var x = 10;
  var innerFunc = function () {
    console.log(x);
  };
  innerFunc();
}

outerFunc(); // 10
```

- 외부함수는 외부함수의 지역변수를 사용하는 내부함수가 소멸될 때까지 소멸되지 않음

```jsx
function outerFunc() {
  var x = 10;
  var innerFunc = function () {
    console.log(x);
  };
  return innerFunc;
}

/**
 *  함수 outerFunc를 호출하면 내부 함수 innerFunc가 반환된다.
 *  그리고 함수 outerFunc의 실행 컨텍스트는 소멸한다.
 */
var inner = outerFunc();
inner(); // 10
```

## 화살표 함수

- 화살표(=>)를 사용하여 보다 간략한 방법으로 함수를 선언
- 왜 쓰냐? `this` 를 다루기 쉬워서 → 무조건 상위 스코프의 this를 가리킴.. (뭔소리)

```jsx
// ES5
var pow = arr.map(function (x) {
  // x는 요소값
  return x * x;
});
```

```jsx
// ES6
const pow = arr.map((x) => x * x);
```

## 스코프

### let, const (ES6)

- 함수 바깥에서 접근 불가
- if 바깥에서 접근 불가

```python
function abc(){
  if(true){
    let a = 5
    }
  console.log(a)
}
abc()

VM3146:5 Uncaught ReferenceError: a is not defined
    at abc (<anonymous>:5:14)
    at <anonymous>:1:1
```

### var

- 함수 바깥에서 접근 불가
- if 바깥에서 접근 가능

```python
function abc(){
  if(true){
    var a = 5
    }
  console.log(a)
}
abc()
5 //결과
console.log(a)
VM482:1 Uncaught ReferenceError: a is not defined
    at <anonymous>:1:13
```
