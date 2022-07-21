## 704.二分查找

升序且不重复的整型数组 nums 和 一个目标值 target ，找出目标值在数组中的下标，不存在则返回 -1

```bash
  输入 : nums = [-1,0,3,5,9,12], target = 9
  输出 : 4
```

```js
  var search = function(nums,target){
    let left = 0 , right = nums.length - 1
    while(left <= right) {
      let mid = Math.floor((left+right) / 2)
      if(nums[mid] > target) {
        right = mid - 1
      }else if(nums[mid] < target) {
        left = mid + 1
      }else {
        return mid
      }
    }
    return -1
  }
```

**注意点: 左右下标**
1. 如果right是长度 - 1 , 而且 left <= right , 那更新操作 right = mid - 1 
2. 如果right不减一,则left < right , 更新操作 right = mid 
3. 返回的结果是 mid 
