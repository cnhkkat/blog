## 全排列
返回字符串的全部排列组合，以数组的形式
```js
permute('abc')
//输出
['abc','acb','bac','bca','cab','cba']
```

**思路：**
- 使用for...of 循环string
- 使用递归，在一个函数中再次调用该函数
- 根据退出递归的条件，有两种思路

## 思路一
- 创建函数p('')，初始参数是''，循环abc，判断是否在p()的参数里，不在就递归调用p()，把当前循环的字符拼接上一次的参数当做参数。

比如一开始是p('')，
第一次循环abc，判断a是否在''里，不在则p(a)
第二次循环abc，判断a是否在'a'里，在则继续循环，判断b是否在'a'里，不在则p(ab)，
第三次循环abc，判断a是否在'ab'里，在，b也在，c不在则p(abc)
此时p(abc)参数的长度等于string的长度，push到结果数组中，然后return。这里的return是跳出最后这次的循环。所以会返回到第二次循环中。
第二次循环abc时判断到b，执行完了p(ab)。接着判断c是否在'a'中，不在则p(ac)。然后接着循环abc，判断a是否在'ac'中，在，b不在则p(acb)。此时acb长度等于string，所以push到结果数组中，然后return到第一次循环中。
第一次循环abc时判断到a，执行完了p(a)。所以接着判断b是否在''中，不在则p(b)。然后接着循环abc，过程和上面类似，总的来说判断a、b、c，先p(ac)然后再p(ca)。先后push bac、bca。最后return到c开头的了，然后重复以上步骤，cab、cba。

1. p('') 判断a在不在参数里 不在则p(a)
2. p(a) a在 b不在
3. p(ab) a在 b在 c不在
4. p(abc) return回3 执行完了p(ab) 就继续执行还没执行完的p(a)回到2 此时a在 b在 c不在 
5. p(ac) a在 b不在 
6. p(acb) return 到p(ac)执行完了 就回到 1 , b 不在 
7. p(b) a不在
8. p(ba) a在 b在 c不在
9. p(bac) return回7 p(b) b在 c不在
10. p(bc) b在 c在 a不在
11. p(bca) return回7 再回到 1 
...

```js
const permute = (string)=>{
  const res = []
  const p = (path) => {
    if(path.length === string.length){
      res.push(path)
      return
    }
    for(let s of string){
      if(!path.includes(s)){
        p(path + s)
      }
    }
  }
  p('')
  return res
}
console.log(permute('abc'))
```
**不同点**
1. for s of 的是 string ，每次调用p(path) 判断的是 遍历的string是否在path里，不在则往path 加 s 。
2. 递归结束的条件是path.length === string.length 

## 思路二
- 不用新建一个函数
第一次调用p(abc):
const res = []
- for s of abc 
  - s = a , filter(abc=>abc !== a) 得到去掉s后的数组 bc, 然后 p(bc) 
    - for s of bc
      - s = b , filter(bc) 得到 p(c)
    if(c.length === 1) return ['c']
    对于函数返回值进行循环，push(s+返回值) 所以 res.push('bc')
      - s = c ,filter(bc) 得到 p(b) 
    同上进行判断return ['b']
    所以此时p(bc)的返回值是有两个，然后循环push，此时 res = ['bc','cb']。
  此时p(bc)的返回值是['bc','cb']，也进行循环push, 此时res.push('abc','acb')
  - s = b , p(ac)
    - for s of ac
      - s = a , p(c),return ['c']
        res.push('ac')
      - s = c , p(a),return ['a']
        res.push('ca')
    res.push('bac','bca')
  - s = c ...同上
  最后res = [ 'abc', 'acb', 'bac', 'bca', 'cab', 'cba' ]
return res 

**不同点**
1. for s of string , p(path)的path是  s不在string里的
2. 调用数组的filter方法，要先把string转为数组，通过string.split('')转。
然后再通过arr.join('')转回string，才能p(string)。    
```js
const permute = (string)=>{
  const res = []
  if(string.length === 1) {
    return [string]
  }
  for(let s of string) {
    const path = string.split('').filter(str => str !== s).join('')
    permute(path).map(item => {
      res.push(s + item)
    })
  }
  return res
}
console.log(permute('abc'))
```

