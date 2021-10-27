## Geolocation

- 웹 애플리케이션에 위치 정보를 제공

```javascript
<script>
    const arrow = document.querySelector('.arrow'); // 변수 선언
    const speed = document.querySelector('.speed-value');

    navigator.geolocation.watchPosition((data) => {  // 현재 위치, 속도
      console.log(data);
      speed.textContent = data.coords.speed;  // 화면에 속도 표시
      arrow.style.transform = `rotate(${data.coords.heading}deg)`;  // 나침반 방향 회전하도록
    }, (err) => {
      console.error(err);
    });
</script>
```

![image-20211027154416467](C:\Users\j2woo\AppData\Roaming\Typora\typora-user-images\image-20211027154416467.png)

: 크롬 상에서 작동이 안되고 Mac에서 작동이 되는 것 같습니다. 작동하면 방향, 속도에 표시가 되는 기능입니다

- `watchPosition()`

  ```javascript
  navigator.geolocation.watchPosition(success[, error[, options]])
  ```

  - success
    - `latitude` : 위도
    - `longitude` : 경도
    - `altitude` : 고도
    - `accuracy` : 위도, 경도 정확도
    - `altitudeAccuracy` : 고도 정확도
    - `heading` : 이동 방향
  - error
    - `PERMISSION_DENIED`
    - `POSITION_UNAVAILABLE`
    - `TIMEOUT`
  - options
    - `enableHighAccuracy` : 위치 정확도
    - `timeout` 
    - `maximumAge` : 위치 갱신 시간 -> 기본값 0 : 항상 현재 위치

