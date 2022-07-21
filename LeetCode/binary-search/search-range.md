## 34.排序数组中查找元素的第一个位置和最后一个位置

给定**升序数组**和目标值，要求 时间复杂度为 O(log n) ，找不到返回 [-1,-1]

```bash
输入：nums = [5,7,7,8,8,10], target = 8
输出：[3,4]
```

### 用时 56ms
```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var searchRange = function(nums, target) {
    let leftBorder = binarySearch(nums,target)
    let rightBorder = binarySearch(nums,target+1) 
    if(nums[leftBorder] !== target ) return [-1,-1]
    return [leftBorder,rightBorder - 1]
};
var binarySearch = function(nums,target) {
    let left = 0 ,right = nums.length - 1
    while(left<=right){
        let mid =  Math.floor((left+right)/2)
        if(nums[mid]>=target){
            right = mid-1
        }else if(nums[mid]<target){
            left = mid+1
        }
    }
    return left
}
```

**巧法**
1. 最后一个位置可以看做是 元素+1 的位置 -1 ，就是找到第一个大于元素的位置，然后-1
2. 返回 left 是元素的第一个位置 或者 按照排序应该插入的位置 [搜索插入的位置](./search-insert.md)
3. nums[leftBorder] !== target 判断找没找到
4. nums[mid]>=target 加上 = 是为了找到第一个元素 

### 官方解法
官方解法是leftIdx是lower为true时，找到第一个大于等于target的下标，lower为false时找到第一个大于target的下标。也就是说，找第一个元素要 >= ,找第一个大于元素的元素要 > 。返回的是mid的值。(返回left也可以)
```js
const binarySearch = (nums, target, lower) => {
    let left = 0, right = nums.length - 1, ans = nums.length;
    while (left <= right) {
        const mid = Math.floor((left + right) / 2);
        if (nums[mid] > target || (lower && nums[mid] >= target)) {
            right = mid - 1;
            ans = mid;
        } else {
            left = mid + 1;
        }
    }
    return ans;
}

var searchRange = function(nums, target) {
    let ans = [-1, -1];
    const leftIdx = binarySearch(nums, target, true);
    const rightIdx = binarySearch(nums, target, false) - 1;
    if (leftIdx <= rightIdx && 
        rightIdx < nums.length && 
        nums[leftIdx] === target && nums[rightIdx] === target) {
            ans = [leftIdx, rightIdx];
    } 
    return ans;
};
```


### 用时 48ms 双指针
```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var searchRange = function(nums, target) {
    var len = nums.length;
    var left = 0;
    var right = len - 1;
    var index = -1;
    while(left <= right){
        var mid = left + Math.floor((right - left) / 2);
        if(target > nums[mid]){
            left = mid + 1;
        }else if(target < nums[mid]){
            right = mid - 1;
        }else{
            index = mid;
            break;
        }
    }
    if(index === -1){
        return[-1, -1];
    }
    left = right = index;
    while(left !== 0 && nums[left - 1] === target){
        left--;
    }
    while(right !== len && nums[right + 1] === target){
        right++;
    }
    return [left, right];
};
```