## **Event Capture, Propagation, Bubbling and Once**

```javascript
<script>
  const divs = document.querySelectorAll('div');  // 상수 선언
  const button = document.querySelector('button');

  function logText(e) {  // 현재 클래스 값을 콘솔에 출력
    console.log(this.classList.value);
    // e.stopPropagation(); // 버블링 금지
    // console.log(this);
  }

  divs.forEach(div => div.addEventListener('click', logText, {  // div 클릭 시 최하위부터 1회만 실행
    // capture: true // 이벤트 하향 전파
    capture: false,  // 이벤트 상향 전파
    once: true  // `click` 이벤트 1회만 실행
  }));

  button.addEventListener('click', () => {  // 버튼 클릭 시 콘솔에 'Click!!!' 메세지 ㅜㄹ력
    console.log('Click!!!');
  }, {
    once: true
  });
</script>
```

![img](https://k.kakaocdn.net/dn/cjN0qP/btqD1rsCw4G/x9j1eCyoCGBen29cHIdU80/img.png)

![JavaScript30-Challenge-25-1](https://t1.daumcdn.net/cfile/tistory/99473F415FB2050E21)

### Capture

- 특정 element에서 event 발생 시 하위 element로 이벤트 전파 (Propagation)

- 이벤트가 하위 요소로 전파되는 단계

  ```javascript
  divs.forEach(div => div.addEventListener('click', logText, {
      capture: false,
      once: true
    }));
  ```

  - `capture: true` : 그림 상에서 one -> two -> three



### Bubbling

- 특정 element에서 event 발생 시 상위 element로 이벤트 전파 (Propagation)
- `capture: false` : 그림 상에서 three -> two -> one



### Propagation

- `event.stopPropagation()` -> Bubbling 중단



### Once

- 한 번 작동 후 `addEventListener` 제거되어 추후 작동 X, 1회만 실행