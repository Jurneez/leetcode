# 盛最多水的容器 


### 题目
[leetcode链接 盛最多水的容器 (11)](https://leetcode.cn/problems/container-with-most-water/)

```
给定一个长度为 n 的整数数组 height 。有 n 条垂线，第 i 条线的两个端点是 (i, 0) 和 (i, height[i]) 。
找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。
返回容器可以储存的最大水量。
说明：你不能倾斜容器。
```

示例 1：
<img src="container-with-most-water01.jpeg" height="200">
```
输入：[1,8,6,2,5,4,8,3,7]
输出：49 
解释：图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。
```

提示：
```
1. n == height.length
2. 2 <= n <= 105
3. 0 <= height[i] <= 104
```
### 解题说明
```
以第一题为例：
1. 在初始时，左右指针分别指向数组的左右两端，它们可以容纳的水量为 min⁡(1,7)∗8=8\min(1, 7) * 8 = 8min(1,7)∗8=8。
2. 移动一个指针：
    a. 容纳的水量 = 两个指针指向的数字中较小值∗指针之间的距离
    b. 如果移动数字较大的那个指针，那么前者「两个指针指向的数字中较小值」不会增加，后者「指针之间的距离」会减小，那么这个乘积会减小
    c. 所以，移动较小的值。
```

### go实现
```go
unc maxArea(height []int) int {
	min := func(num1, num2 int) int {
		if num1 < num2 {
			return num1
		}
		return num2
	}

	left_index, right_index := 0, len(height)-1
	max := 0
    
	for right_index > left_index {
		max_t := min(height[left_index], height[right_index]) * (right_index - left_index)
		if max < max_t {
			max = max_t
		}

		if height[left_index] < height[right_index] {
			left_index++
		} else {
			right_index--
		}


	}

	return max
}
```