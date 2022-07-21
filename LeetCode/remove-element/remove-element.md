## 27.移除元素 

给定数组 nums 和 一个值 val，**原地** 移除所有数值等于 val 的元素，并返回移除后数组的新长度。

```bash
输入：nums = [3,2,2,3], val = 3
输出：2, nums = [2,2]
解释：函数应该返回新的长度 2, 并且 nums 中的前两个元素均为 2。你不需要考虑数组中超出新长度后面的元素。例如，函数返回的新长度为 2 ，而 nums = [2,2,3,3] 或 nums = [2,2,0,0]，也会被视作正确答案。
```

### 不用api的解法 双指针
```js
/**
 * @param {number[]} nums
 * @param {number} val
 * @return {number}
 */
var removeElement = function(nums, val) {
    let j = 0
    for(let i = 0;i < nums.length;i++){
        if(nums[i] !== val){
            nums[j] = nums[i]
            j++
        }
    }
    return j
};
```

- 快指针: i
- 慢指针: j,新数组的下标
- **数组元素只能覆盖，不能删除**


## 使用api nums.splice(i,1) 直接删除原数组的元素

## 26.删除有序数组重复的元素
给你一个 升序排列 的数组 nums ，原地 删除重复出现的元素，使每个元素 只出现一次 ，返回删除后数组的新长度。元素的 相对顺序 应该保持 一致 

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var removeDuplicates = function(nums) {
    let j = 1;
    if(nums.length === 1) return 1
    for(let i = 1;i<nums.length;i++){
        if(nums[i] !== nums[i-1]){
            nums[k] = nums[i]
            j++
        }
    }
    return j
};
```

## 283.移动零

给定一个数组 nums，编写一个函数**将所有 0 移动到数组的末尾**，同时保持非零元素的相对顺序。

请注意 ，必须在不复制数组的情况下原地对数组进行操作。

```bash
输入: nums = [0,1,0,3,12]
输出: [1,3,12,0,0]
```

```js
/**
 * @param {number[]} nums
 * @return {void} Do not return anything, modify nums in-place instead.
 */
var moveZeroes = function(nums) {
    let j = 0
    for(let i=0;i<nums.length;i++){
        if(nums[i]!==0){
            if(i > j){
                nums[j]=nums[i]
                nums[i]=0
            }
            j++
        }
    }
};
```

- `if(i > j)` 避免第一个元素是非零的情况
- j++ 写在 `if(i > j)`  外
- 另一种解法是 像移除元素 得到 j ，然后再循环 `i=j;i<nums.length;`把后面的赋值为0 `nums[i]=0`