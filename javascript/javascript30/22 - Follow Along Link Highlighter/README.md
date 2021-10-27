### 효과는 HTML에 생성 해야될까?

- X. 동적으로 제어해야 하니까 JS로 만들어야 한다.

```jsx
const highlight = document.createElement("span");
highlight.classList.add("highlight");
document.body.appendChild(highlight);
```

### 어떻게 하면 컴포넌트 크기에 맞게 효과를 변화시킬 수 있을까?

- `getBoundingClientRect()` - 컴포넌트의 크기 속성을 가져오자!

```jsx
const linkCoords = this.getBoundingClientRect(); // this의 장점!!

const coords = {
  width: linkCoords.width,
  height: linkCoords.height,
  top: linkCoords.top + window.scrollY,
  left: linkCoords.left + window.scrollX,
};

highlight.style.width = `${coords.width}px`;
highlight.style.height = `${coords.height}px`;
highlight.style.transform = `translate(${coords.left}px, ${coords.top}px)`;
```

### 어떻게 스르륵 따라오게 인터랙션할까?

```jsx
highlight.style.width = `${coords.width}px`;
highlight.style.height = `${coords.height}px`;
highlight.style.transform = `translate(${coords.left}px, ${coords.top}px)`;
```

스타일 변경들을 트랜지션으로 처리하면 된다.

```css
.highlight {
  transition: all 0.2s;
}
```
