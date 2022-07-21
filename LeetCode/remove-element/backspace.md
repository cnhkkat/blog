## 844. 比较含退格的字符串

给定 s 和 t 两个字符串，如果两者相等，返回 true 。# 代表退格字符。

注意：如果对空文本输入退格字符，文本继续为空。

### 双指针 从后往前 🤔
```js
var backspaceCompare = function (S, T) {
    let i1 = S.length - 1;
    let i2 = T.length - 1;
    while (i1 >= 0 || i2 >= 0) {
        // 跳过#字符
        if (S[i1] == '#') {
            let num = 1;
            while (i1 >= 0 && S[--i1] == '#') num++;
            // 删除过程中，如果遇到#
            while (num != 0) {
                i1--;
                if (S[i1] == '#') num++;
                else num--;
            }
        }
        if (T[i2] == '#') {
            let num = 1;
            while (i2 >= 0 && T[--i2] == '#') num++;
            while (num != 0) {
                i2--;
                if (T[i2] == '#') num++;
                else num--;
            }
        }
        // 实现比较
        if (S[i1] == '#' || T[i2] == '#') continue;
        else {
            if (S[i1] != T[i2]) return false;
            i1--;
            i2--;
        }
    }
    if (i1 < 0 && i2 < 0) return true;
    else return false;
};
```

### 转成数组 使用 栈 的 push pop 方法
```js
var backspaceCompare = function(s, t) {
    return stack(s) === stack(t)
};

var stack = function(s){
    let sArr=[]
    for(let k of s){
        if(k!=='#'){
            sArr.push(k)
        }else if(sArr.length !==0){
            sArr.pop(k)
        }
    }
    return sArr.join('')
}
```

### replace 正则
```js
    while(str.indexOf('#')!=-1)
        str= str.replace(/.?\#/,'');
    return str;
```