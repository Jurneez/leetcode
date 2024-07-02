# 奇偶数分组

华为面试现场算法题

## 题目
```
给定一个非负整数数组 A，返回一个由 A 的所有偶数元素组成的数组，后面跟 A 的所有奇数元素。
基础版：你可以返回满足此条件的任何数组作为答案。
进阶版：要求在当前数组上原地完成。

示例：
输入：[3,1,6,5,2,4]
进阶版输出：[4,2,6,5,1,3]
基础版输出：[2,4,6,1,3,5]，[4,6,2,3,1,5] 等等 也会被接受
```

## go代码实现
```
func handle(nums []int) []int {
	i, j := 0, len(nums)-1
	for i < j {
		if nums[i]%2 == 1 && nums[j]%2 == 0 {
			nums[i], nums[j] = nums[j], nums[i]
			i++
			j--
			continue
		}
		if nums[i]%2 == 0 {
			i++
		}
		if nums[j]%2 == 1 {
			j--
		}
	}

	return nums
}
```