**Console** : 로딩된 웹페이지의 에러를 확인하거나 자바스크립트 소스코드에 작성한 `console.log()` 메서드의 실행 결과를 확인할 수 있다.

<br/>

### **Regular**

일반적으로 콘솔 창에서 확인하고 싶을 때는 `console.log()` 를 이용하면 된다.

```jsx
console.log('hello');
```

<br/>

### **Interpolated**

string을 문자열 안에 넣고 싶은 경우 %s 를 이용한다.

```jsx
console.log('Hello I am a %s string', '💩')
```

<br/>

### **템플릿 리터럴**

템플릿 리터럴은 내장된 표현식을 허용하는 문자열 리터럴이다. 템플릿 리터럴은 백틱`을 이용해서 표현식을 감싸줘야 하고, 사용하고자 하는 변수는`${}` 안에 쓰면 된다.

> 템플릿 리터럴을 이용

```jsx
const poo = '💩'
console.log(`Hello I an a ${poo} string`);
```

<br/>

### **Styled**

`%c` 를 이용하면 콘솔 텍스트에 스타일을 적용할 수 있다.

```jsx
console.log('%c I am some great text', 'font-size:50px; background:red; text-shadow: 10px 10px 0 blue')
```

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/519f5eea-beb8-4987-8289-2c637a51b3ca/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211026T144332Z&X-Amz-Expires=86400&X-Amz-Signature=d2bb9747f60ea79abbb17db6742ec3dcd0fe5537123e914d7834dc0e06935b2f&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

<br/>

### W**arning/Error/Info**

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/fcef64bd-43e0-4ab7-b567-ca7179f28d45/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211026T144346Z&X-Amz-Expires=86400&X-Amz-Signature=41a83358fbfdc5263cf23f0ef5e52848ce5b01f2fa845e7b0873fe71f3d78c7b&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

```jsx
// warning!
console.warn('OH NOOO');

// Error :|
console.error('Shit!');

// Info
console.info('Crocodiles eat 3-4 people per year');
```

<br/>

### **Testing : `console.assert()`**

- 주어진 가정이 거짓인 경우 콘솔에 오류 메시지를 출력. 참인 경우 아무것도 하지 않는다.
- if 문을 사용하지 않고 콘솔 창에서 간단히 확인할 때 사용한다.

> `<p onClick="makeGreen()">×BREAK×DOWN×</p>`

```jsx
const p = document.querySelector('p');
console.assert(p.classList.contains('ouch'), 'That is wrong!');
```

<br/>

### **Clearing : `console.clear()`**

콘솔 창을 깨끗하게 만들 때 사용한다.

```jsx
console.clear()
```

<br/>

### **Viewing DOM Elements**

DOM Elements를 보기 위해서 객체는 dir로 확인하고, 나머지 경우는 log로 확인하면 된다.

```jsx
console.log(p);
console.dir(p);
```

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/71f35fe3-4ddd-490b-a582-bdc215894c7f/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211026T144402Z&X-Amz-Expires=86400&X-Amz-Signature=0633a3df735a589159fc43425be9d48a35038cc048ed6b1550cf24ebce33b6ce&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

<br/>

### **Grouping together**

`console.group` 은 한 번 반복할 때마다 그룹으로 묶어서 확인하고 싶을 때 사용한다.

- 묶고 싶은 곳의 시작에서 `console.group`, 묶고 싶은 곳의 마지막에서 `console.groupEnd` 를 사용한다
- `console.group` 대신 `console.groupCollapsed`를 사용하면 드롭다운이 닫혀 있는 채로 콘솔에서 찍히게 되어 콘솔 창이 더 깔끔해보인다.

```jsx
const dogs = [{ name: 'Snickers', age: 2 }, { name: 'hugo', age: 8 }];

dogs.forEach(dog => {
  console.groupCollapsed(`${dog.name}`);
  console.log(`This is ${dog.name}`);
  console.log(`${dog.name} is ${dog.age} years old`);
  console.log(`${dog.name} is ${dog.age * 7} dog years old`);
  console.groupEnd(`${dog.name}`);
});
```

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/fe00dd56-8e37-4266-a2c0-794e82842f4b/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211026T144412Z&X-Amz-Expires=86400&X-Amz-Signature=acbebc237889e2e98b2081145665c226cf1c831aa8725125172ff9b57a20c256&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

<br/>

### **Counting : `console.count()`**

- 같은 로그가 몇 번째 출력되고 있는지를 확인할 수 있다.
- 카운트 초기화 : `console.countReset()`

```jsx
console.count('Wes');
console.count('Wes');
console.count('Steve');
console.count('Steve');
console.count('Wes');
console.count('Steve');
console.count('Wes');
console.count('Steve');
console.count('Steve');
console.count('Steve');
console.count('Steve');
console.count('Steve');
```

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/7db9a32e-55c2-430b-bbeb-4c362d4d0c9e/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211026T144425Z&X-Amz-Expires=86400&X-Amz-Signature=c789832161b67d059ce0cf1a3fc84c4e59e0b19e0df9ca201ed57376ffd97bf6&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

<br/>

### **Timing**

- `console.time`을 이용하면 실행되는데 걸리는 시간을 출력할 수 있다.
- 시간을 재기 시작하고 싶은 곳에 `console.time`, 시간 재는 것을 멈추고 싶은 곳에 `console.timeEnd`를 사용한다.

> fetch로 데이터를 가져오는데 걸리는 시간을 출력

```jsx
console.time('fetching data');
fetch('<https://api.github.com/users/wesbos>')
  .then(data => data.json())
  .then(data => {
  console.timeEnd('fetching data');
  console.log(data);
});
```

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/7db9a32e-55c2-430b-bbeb-4c362d4d0c9e/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211026T144450Z&X-Amz-Expires=86400&X-Amz-Signature=29d652167a82f792d84176df2f58f8dc935f9d5ff1081bce411c5ebc542a9c73&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

<br/>

### Table

```jsx
console.table(dogs);
```

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/fcd9779d-e15a-4c9c-b8af-32a18e8806a9/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211026T144456Z&X-Amz-Expires=86400&X-Amz-Signature=197d7da47f572fb4407de05628a6fe4541cb5241c770937882b2372358472f88&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)