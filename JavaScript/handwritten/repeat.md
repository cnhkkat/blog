## 实现repeat方法

## 思路一
```js
function repeat(func, times, wait) {
  return (...args) => {
    let count = 0
    let timer = setInterval(() => {
      func(...args)
      count++
      if (count == times) clearInterval(timer)
    }, wait)
  }
}

repeat(console.log, 3, 3000)('hi')
// 每隔3秒输出hi，输出3次
```
**要点**
- 返回一个函数，接收参数
- 用`setInterval` + `count`，如果达到重复次数，就`clearInterval(timer)`


## 思路二

```js
  for (let i = 0; i < times; i++) {
    setTimeout(() => {
      func(...args)
    }, wait * i)
  }
```
**要点**
- `for` + setTimeout(()=>fn(), `wait * i`)