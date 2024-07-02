# 单调栈：移掉 K 位数字 二

#### 题目
给定数组nums，移除K位数字，使剩余数据组成的数字最大，数组中各数据的相对位置不变。

例如：
```
nums = [9, 1, 2, 5, 8, 3], k = 2
result = [9,5,8,3]
```

##### golang实现
```go
func maxValue(nums []int, m int) []int {
	stack := make([]int, 0)
	for _, num := range nums {
		for len(stack) > 0 && m > 0 && num > stack[len(stack)-1] {
			stack = stack[:len(stack)-1]
			m--
		}

		stack = append(stack, num)
	}

	stack = stack[:len(stack)-m]

	return stack
}
```