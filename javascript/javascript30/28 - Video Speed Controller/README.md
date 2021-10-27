```jsx
function handleMove(e) {
  const y = e.pageY - this.offsetTop;
  const percent = y / this.offsetHeight;
  const min = 0.4;
  const max = 4;
  const height = Math.round(percent * 100) + "%";
  const playbackRate = percent * (max - min) + min;
  bar.style.height = height;
  bar.textContent = playbackRate.toFixed(2) + "×";
  video.playbackRate = playbackRate;
}
```

- `pageY` : 화면 상의 마우스 커서 y 축 위치

### 기하 프로퍼티

[https://ko.javascript.info/size-and-scroll](https://ko.javascript.info/size-and-scroll)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d2bc022c-d2d5-4c35-a031-672ebb1cb68a/Untitled.png)
