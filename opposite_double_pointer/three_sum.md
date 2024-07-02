# 三数之和

### 题目
[leetcode链接 三数之和 (15)）](https://leetcode.cn/problems/3sum/description/)

```
给你一个整数数组 nums ，判断是否存在三元组 [nums[i], nums[j], nums[k]] 满足：
    i != j、i != k 且 j != k ，
    同时还满足 nums[i] + nums[j] + nums[k] == 0 。

请你返回所有和为 0 且不重复的三元组。
注意：答案中不可以包含重复的三元组。

示例 1：
输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]
解释：
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0 。
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0 。
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0 。
不同的三元组是 [-1,0,1] 和 [-1,-1,2] 。
注意，输出的顺序和三元组的顺序并不重要。

示例 2：
输入：nums = [0,1,1]
输出：[]
解释：唯一可能的三元组和不为 0 。

示例 3：
输入：nums = [0,0,0]
输出：[[0,0,0]]
解释：唯一可能的三元组和为 0 。
 
提示：
1. 3 <= nums.length <= 3000
2. -105 <= nums[i] <= 105
```

### 解题说明
```
数组无序，可重复，结果不确定。 题目中，求三个数之和为0的数组，可以假定其中一个数的负数形式为target，求其他两个数相加等于target。可采用相向双指针的方法。 解题过程：
1. 题目中输入的数组无序，所以采用相向双指针前，需要先对数组进行排序。使：i < j < k,num[i] < num[j] < num[k]
2. for循环设置每一个元素为target = -1 * (num[i]),只需要求剩下数据两数相加是否等于target即可。
```

### go实现
```go
func threeSum(nums []int) [][]int {
	sort.Ints(nums)

	res := make([][]int, 0)
	for i := 0; i < len(nums)-2; i++ {
		if i > 0 && nums[i] == nums[i-1] { // 连续重复的数据，不需要再次计算
			continue
		}

		target := 0 - nums[i]
		if target < 0 { // 如果nums[i] > 0,那么，num[j] 和 nums[k] 一定也大于0，后续未计算到的数据也都是大于0的，所以直接break
			break
		}

		j, k := i+1, len(nums)-1
		for j < k {
			t := nums[j] + nums[k]
			if t > target {
				k--
			} else if t < target {
				j++
			} else {
				res = append(res, []int{nums[i], nums[j], nums[k]})
				j++ // 结果不止一个，查出一个符合条件的数据后，继续查找下一个符合条件的数组
				for j < k {
					if nums[j] > nums[j-1] { // 连续重复的数据，不需要再次计算
						break
					} else {
						j++
					}
				}

			}
		}
	}

	return res
}
```