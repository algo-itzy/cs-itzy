# 18. Adding Up Times with Reduce

### NodeList & **map() : Error**

```javascript
const timeNodes = document.querySelectorAll('[data-time]');

const seconds = timeNodes.map(node => node.dataset.time);
```

위와 같이 실행하게 되면

`Uncaught TypeError: timeNodes.map is not a function` Type에러가 발생.

- **map()**은 **배열 내의** 모든 요소 각각에 대하여 주어진 함수를 호출한 결과를 모아 새로운 배열을 반환`Array.prototype.map()`
- **즉, 배열이 아니라서 Type Error가 발생한 것이다.**

NodeList들을 배열로 바꿔주기 위해 `...`나 `Array.from()`을 사용해 배열로 만들어준다.

```javascript
const seconds = timeNodes
    .map(node => node.dataset.time)
    .map(timeCode => {
    const [mins, secs] = timeCode.split(':').map(parseFloat);
    return (mins*60) + secs;
    // console.log(mins, secs);
	})
    .reduce((total, vidSeconds) => total + vidSeconds);
```

- `split(':')`
- `parseFloat` : 문자열을 실수로 반환

### reduce

- reduce function은 누산기로서 배열의 각 요소값들을 이용해 하나의 단일값을 만들어준다.
- 파라미터로 받는 콜백함수는 두 개의 파라미터를 받는데, 첫 번째 인자는 accumulator, 즉 콜백의 반환값을 의미하고, 두 번째 인자는 해당 인덱스의 값(currentValue)를 의미한다.
- [MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)
- [reduce 설명](https://niceman.tistory.com/79)