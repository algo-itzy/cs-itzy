# Mouse Move Shadow

### contenteditable 속성

html 태그에 `contenteditable`을 속성을 `true`로 설정하면 `input` 태그처럼 수정할 수 있다. 원래는 `<h1 contenteditable="true">` 처럼 속성을 줘야 하지만 `true`는 명시하지 않고 속성명만 적어도 된다. `h` 태그처럼 텍스트 태그에만 한정되있는게 아니고 `div` 등에도 걸 수 있다.

<br/>

### `{}`로 한꺼번에 선언하기 : 비구조화 할당 (Destructuring Assignment)

`hero` 객체에는 `offsetWidth`, `offsetHeight` 값이 있는데, 이를 `width`, `height`라는 변수로 선언하고자 할 때

```jsx
const width = hero.offsetWidth
const height = hero.offsetHeight
```

ES6 문법을 사용하면 가독성과 코드 낭비를 한번에 해결할 수 있다.

```jsx
const { offsetWidth: width, offsetHeight: height } = hero;
```

<br/>

### offsetWidth / offsetHeight

margin을 제외한, padding값, border값까지 계산한 너비와 높이

<br/>

### css - textshadow

```css
text-shadow: offset-x offset-y blur-radius color | none | initial | inherit
```

- offset-x: 그림자의 수평 거리
- offset-y: 그림자의 수직 거리
- blur-radius: 흐림 정도(기본 0)
- color: 색상 (기본 브라우저 기본값)
- none: 그림자 효과 없애기
- initial: 기본값으로
- inherit: 부모의 속성값 상속

<br/>

### **마우스의 위치 파악하기**

- hero의 크기를 알기 위해 ⇒ `hero.offsetWidth` `hero.offsetHeight`
- 현재 마우스의 위치를 알기 위해 ⇒ `e.offsetX` `e.offsetY`
- `this` 는 hero를 가리키고, `e.target` 은 text 밖에 있으면 hero이고, text 안에 있으면 h1이다.
- 따라서 `e.target` 이 h1일 때는 offsetX와 offsetY 값이 hero일 때와 다르기 때문에 x는 `e.target.offsetLeft` 를, y는 `e.target.offsetTop` 를 더해줘야 한다.

```jsx
  const hero = document.querySelector('.hero');
  const text = hero.querySelector('h1');
  const walk = 500; // 500px

  function shadow(e) {
    const { offsetWidth: width, offsetHeight: height } = hero;
		// const width = hero.offsetWidth
		// const height = hero.offsetHeight

    let { offsetX: x, offsetY: y } = e;

    if (this !== e.target) {
      x = x + e.target.offsetLeft;
      y = y + e.target.offsetTop;
    }
  }

	hero.addEventListener('mousemove', shadow);
```

<br/>

### **마우스의 움직임에 따라 그림자가 움직이도록 하기**

- 처음에는 x, y값만 이용해서 마우스 움직임에 따라 그림자들의 위치를 설정했는데, 이 경우 브라우저의 왼쪽 위를 기준으로 그림자의 위치가 반영돼서 원하는 애니메이션을 만들 수 없었다.
- 따라서 마우스 움직임에 따라 hero를 주변을 맴돌면서 움직이도록 만들기 위해 xWalk와 yWalk값을 만든다

```jsx
  const hero = document.querySelector('.hero');
  const text = hero.querySelector('h1');
  const walk = 500; // 500px

  function shadow(e) {
    const { offsetWidth: width, offsetHeight: height } = hero;
    let { offsetX: x, offsetY: y } = e;

    if (this !== e.target) {
      x = x + e.target.offsetLeft;
      y = y + e.target.offsetTop;
    }

		// 현재 마우스 위치를 div 높이와 너비로 나누고 일정량을 곱한 뒤 반올림해준 값을 변수로 담는다.
    const xWalk = Math.round((x / width * walk) - (walk / 2));
    const yWalk = Math.round((y / height * walk) - (walk / 2));

		// 그림자가 어떻게 생길지 설정해준다.
    text.style.textShadow = `
      ${xWalk}px ${yWalk}px 0 rgba(255,0,255,0.7),
      ${xWalk * -1}px ${yWalk}px 0 rgba(0,255,255,0.7),
      ${yWalk}px ${xWalk * -1}px 0 rgba(0,255,0,0.7),
      ${yWalk * -1}px ${xWalk}px 0 rgba(0,0,255,0.7)
    `;

  }

  hero.addEventListener('mousemove', shadow);
```

