## 웹에 카메라 넣는 법

```jsx
function getVideo() {
  navigator.mediaDevices
    .getUserMedia({ video: true, audio: false })
    .then((localMediaStream) => {
      // video.src = window.URL.createObjectURL(localMediaStream);
      video.srcObject = localMediaStream;
      video.play();
    })
    .catch((err) => {
      console.error(`no cam`, err);
    });
}
```

## 픽셀 데이터

```jsx
let pixels = ctx.getImageData(0, 0, width, height);
```

![image](https://user-images.githubusercontent.com/43740455/138905858-60ed159b-4e0a-4d22-9d66-77aa73b42aaa.png)


### Fliter - Red Effect

![image](https://user-images.githubusercontent.com/43740455/138905940-99a2205b-8590-4efc-a181-f8b487b62abb.png)

```jsx
function redEffect(pixels) {
  for (let i = 0; i < pixels.data.length; i += 4) {
    pixels.data[i + 0] = pixels.data[i + 0] + 200; // RED
    pixels.data[i + 1] = pixels.data[i + 1] - 50; // GREEN
    pixels.data[i + 2] = pixels.data[i + 2] * 0.5; // Blue
  }
  return pixels;
}
```

### Filter - RGB Split

![image](https://user-images.githubusercontent.com/43740455/138905975-0293fba0-9cdd-4e1f-8474-b4ed0e6bcf45.png)


```jsx
function rgbSplit(pixels) {
  for (let i = 0; i < pixels.data.length; i += 4) {
    pixels.data[i - 150] = pixels.data[i + 0]; // RED
    pixels.data[i + 500] = pixels.data[i + 1]; // GREEN
    pixels.data[i - 550] = pixels.data[i + 2]; // Blue
  }
  return pixels;
}
```



### Filter - Afterimage

![image](https://user-images.githubusercontent.com/43740455/138906115-20091c39-beab-4f6f-880c-4fbe2b145fd0.png)

```jsx
function paintToCanvas() {
  const width = video.videoWidth;
  const height = video.videoHeight;
  canvas.width = width;
  canvas.height = height;

  setInterval(() => {
    ctx.drawImage(video, 0, 0, width, height);
    let pixels = ctx.getImageData(0, 0, width, height);

    ctx.globalAlpha = 0.1;

    ctx.putImageData(pixels, 0, 0);
  }, 16);
}
```

### Filter - Chroma key

- 스킵
