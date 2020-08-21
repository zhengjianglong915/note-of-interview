# 背景
最近在学习go, 用 leetCode 练练手


## 1480. Running Sum of 1d Array
see more: https://leetcode.com/problems/running-sum-of-1d-array/

#### 实现

```
func runningSum(nums []int) []int {
    if nil == nums || len(nums) == 0{
        return nil
    }

    for i:=1; i < len(nums); i++ {
        nums[i] += nums[i-1]
    }
}
```
