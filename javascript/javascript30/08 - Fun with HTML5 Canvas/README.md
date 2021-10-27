## Canvas 선택

```jsx
const canvas = document.querySelector("#draw");
const ctx = canvas.getContext("2d");
```

## Canvas 속성

```jsx
canvas.width = window.innerWidth;
canvas.height = window.innerHeight;
ctx.strokeStyle = "#BADA55";
ctx.lineJoin = "round";
ctx.lineCap = "round";
ctx.lineWidth = 100;
```

## 마우스 누를 때만 그리기

```jsx
canvas.addEventListener("mousemove", draw);
canvas.addEventListener("mouseup", () => (isDrawing = false));
canvas.addEventListener("mouseout", () => (isDrawing = false));
```

```jsx
function draw(e) {
  if (!isDrawing) return;
  ...
}
```

## 그리기 함수 (실패작)

```jsx
ctx.beginPath();
// start from
ctx.moveTo(lastX, lastY);
// go to
ctx.lineTo(e.offsetX, e.offsetY);
ctx.stroke();
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0751333a-9d15-4881-8670-941fe038eedb/Untitled.png)

## 그리기 함수 (반만 성공작)

- 마우스 뗐다가 다시 누르면 선이 생긴다.

```jsx
ctx.beginPath();
// start from
ctx.moveTo(lastX, lastY);
// go to
ctx.lineTo(e.offsetX, e.offsetY);
ctx.stroke();
[lastX, lastY] = [e.offsetX, e.offsetY];
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/eb7ef780-f0f6-49f2-a6a0-b0d08e5708e4/Untitled.png)

## 그리기 함수 (성공작)

```jsx
canvas.addEventListener("mousedown", (e) => {
  isDrawing = true;
  [lastX, lastY] = [e.offsetX, e.offsetY];
});
```

## 무지개 브러쉬

```jsx
ctx.strokeStyle = `hsl(${hue}, 100%, 50%)`;
```

### **Why you should use HSL over other colour formats in your CSS**

- HSL은 현실감 있는, 인간 친화적 칼라 모델
  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/62629382-4279-43d5-b4af-c00a71bcd162/Untitled.png)
