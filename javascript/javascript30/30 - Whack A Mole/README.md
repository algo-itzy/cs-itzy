## 두더지 튀어오르게 하는 법

```css
.mole {
  background: url("mole.svg") bottom center no-repeat;
  background-size: 60%;
  position: absolute;
  top: 100%; // 숨은 상태
  width: 100%;
  height: 100%;
  transition: all 0.4s;
}

.hole.up .mole {
  top: 0; // 올라온 상태
}
```

## 두더지를 숨기는 방법

```css
.hole {
  flex: 1 0 33.33%;
  overflow: hidden; // 이것!
  position: relative;
}
```

## 치트를 막는 방법

```jsx
const bonk = (e) => {
  if (!e.isTrusted) return;
  score++;
  this.parentNode.classList.remove("up");
  scoreBoard.textContent = score;
};
```
