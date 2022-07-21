## 69. x的平方根 (367.有效的完全平方数)

69. 给定**非负整数 x** ，返回 x 的算术平方根，结果**保留整数部分**，舍去小数部分，不使用内置函数pow
367. 给定**正整数 num**,如果 num 是一个完全平方数，则返回 true ，否则返回 false 。

```bash
输入：x = 8
输出：2
解释：8 的算术平方根是 2.82842..., 由于返回类型是整数，小数部分将被舍去
```

### 用时 68ms 内存消耗 42.4MB

```js
/**
 * @param {number} x
 * @return {number}
 */
var mySqrt = function(x) {
    let left = 0 , right = Math.floor(x/2)
    if(x<=1) return x
    while(left<=right){
        let mid = left + Math.floor((right - left)/2)
        if(mid * mid < x){
            left = mid + 1
        }else if(mid * mid > x){
            right = mid - 1
        }else {
            return mid
        }
    }
    return right
};
```

**巧法**
1. right = Math.floor(x/2) 可以节约用时，要注意加上判断 x <= 1 不然right = 0 时返回的是0。
2. return right 因为题意是 结果的平方 <= x ，所以二分查找时不能取到 left 值，所以是 right。也就是当 mid * mid > x 时，x的平方根就要 mid - 1 了，也就是right的值。
3. **比 mid 小就返回 right ，比 mid 大就返回 left**

### 用时 88ms 内存消耗 42MB
```js
/**
 * @param {number} x
 * @return {number}
 */
var mySqrt = function(x) {
    let left = 0 , right = Math.floor(x/2),ans = -1
    if(x<=1) return x
    while(left<=right){
        let mid =  Math.floor((left+right)/2)
        if(mid * mid <= x){
            ans = mid
            left = mid + 1
        }else if(mid * mid > x){
            right = mid - 1
        }
    }
    return ans
};
```

1. mid * mid <= x 时 ans = mid