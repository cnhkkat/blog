## 二分查找

升序且不重复的整型数组 nums 和 一个目标值 target ，找出目标值在数组中的下标，不存在则返回 -1

```bash
  输入 : nums = [-1,0,3,5,9,12], target = 9
  输出 : 4
```

```js
  var search = function(nums,target){
    let left = 0 , right = nums.length - 1
    while(left <= right) {
      let mid = left + Math.floor((right - left) / 2)
      if(nums[mid] < target) {
        left = mid + 1
      }else if(nums[mid] > target) {
        right = mid -1
      }else {
        return mid
      }
    }
    return -1
  }
```

**要点**
1. right 是长度 - 1 
2. left <= right是下标默认的从0开始到结尾 ， 求中间数是总数/2 , 但是因为右下标是包含左下标的，所以需要 (right - left) / 2 。因为JS中 3 / 2 = 1.5 ，所以需要向下取整，用 Math.floor() 。如果left一直是 0 ，那可以直接(right - left) / 2 ，但是left会根据target值而变化，比如 left=0，right=5，mid=2，target > nums[mid],left=mid+1=3,此时 5-3=2/2=1,很显然1不在右边，需要加上left=1+3=4才对，所以`算出来的mid=1是在新的范围的第2个元素，此时的下标是要算上左边的个数`，可以直接加left。
3. 返回的结果是 mid 
4. 如果 while(left < right) 那right = mid ，不用 -1 ，因为 <= 就是 -1 的意思。
5. mid < target 变化的是 left 。 < 对应 left 对应 + 1
   如果是 > 那就是 right 对应 -1 

6. 如果是降序，则只需要 < 对应right ， > 对应left