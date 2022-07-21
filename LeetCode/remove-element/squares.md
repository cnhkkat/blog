## 977.有序数组的平方

给定**升序整数数组** nums，返回 每个数字的平方 组成的新数组，按升序排序。

```bash
输入：nums = [-7,-3,2,3,11]
输出：[4,9,9,49,121]
```

### 双指针法
```js
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var sortedSquares = function(nums) {
    let arr =  Array.from(nums).fill(0);
    let length = nums.length-1;
    for(let i = 0,j = length,ans = length; i <= j;){
        if(-nums[i] > nums[j]){
            arr[ans--] = nums[i] * nums[i]
            i++
        }else {
            arr[ans--] = nums[j] * nums[j]
            j--
        }
    }
    return arr
};
```

### 遍历 + sort
```js
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var sortedSquares = function(nums) {
    for(let i=0;i<nums.length;i++)
    {
        nums.forEach((item,index)=>nums[index]=item*item)
        nums.sort((a,b)=>a-b)
        return nums
    }
};
```