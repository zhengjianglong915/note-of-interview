## 1480. Running Sum of 1d Array
see more: https://leetcode.com/problems/running-sum-of-1d-array/

#### 实现

```
func runningSum(nums []int) []int {
    if nil == nums || len(nums) == 0{
        return nil
    }

    var count = 0
    for i, v := range nums {
        count += v
        nums[i] = count
    }

    return nums
}
```
