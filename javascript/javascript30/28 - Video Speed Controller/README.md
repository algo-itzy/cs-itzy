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

![image](https://user-images.githubusercontent.com/43740455/139092400-bec438b1-5486-4dec-afd2-e5a5336c0153.png)
