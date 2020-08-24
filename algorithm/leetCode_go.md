# 背景
最近在学习go, 用 leetCode 练练手


## 1480. Running Sum of 1d Array
see more: https://leetcode.com/problems/running-sum-of-1d-array/

#### 实现

```
func runningSum(nums []int) []int {
    if nil == nums || len(nums) == 0{
        return nums
    }

    for i:=1; i < len(nums); i++ {
        nums[i] += nums[i-1]
    }
    return nums
}
```


##  1470. Shuffle the Array
see more: https://leetcode.com/problems/shuffle-the-array/

#### 实现

```
func shuffle(nums []int, n int) []int {
    if nil == nums || len(nums) == 0 {
        return nums
    }
    
    result := make ([]int, len(nums))
    
    for i:= range nums[:n] {
        result[i * 2], result[i * 2 + 1] = nums[i], nums[i+n]
    }
    
    return result
}
```
