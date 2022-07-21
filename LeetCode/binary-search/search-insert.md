## 35.搜索插入位置

给定**不重复**元素的**升序数组** 和 目标值，找到返回下标，找不到返回按顺序插入的位置

```bash
输入: nums = [1,3,5,6], target = 5
输出: 2
```

### 二分查找法
```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */
var searchInsert = function(nums, target) {
    let left = 0,right=nums.length-1
    while(left<=right){
        let mid = Math.floor((left + right) / 2)
        if(nums[mid] > target){
            right = mid - 1
        }else if (nums[mid]<target){
            left = mid + 1
        }else {
            return mid
        }
    }
    return left 
};
```

**注意点**
1. 因为是不重复，所以退出while循环的条件是
  - 要么找到返回 mid
  - 要么没找到，此时left = right = mid , 此时不管target在mid的左边还是右边,移动指针后都会导致 left > right ，所以left在right的右边，是按顺序插入的位置
2. **比 mid 大就返回 left**


### 暴力解法
```js
var searchInsert = function(nums, target) {
    for(let i = 0 ; i < nums.length ;i++){
        if(nums[i] >= target)
            return i
    }
    return nums.length
};
```

1. 一旦发现大于或者等于target的num[i]，那么i就是我们要的结果
2. 如果target是最大的，或者 nums为空，则返回nums的长度