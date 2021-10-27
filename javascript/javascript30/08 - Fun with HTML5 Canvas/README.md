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

![image](https://user-images.githubusercontent.com/43740455/139089916-4bf3d4e1-a0bf-4a40-a344-d5324386ad5f.png)

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

![image](https://user-images.githubusercontent.com/43740455/139089940-7dca0811-7ad4-476f-a0f7-3dffdc261b67.png)

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

- HSL은 현실감 있는, 인간 친화적 색상 모델

![image](https://user-images.githubusercontent.com/43740455/139089961-f8dfec39-6f68-4f4c-b5c1-014a84b95f79.png)
