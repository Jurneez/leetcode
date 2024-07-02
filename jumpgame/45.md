# 跳跃游戏 II

### 题目
[leetcode 45. 跳跃游戏 II](https://leetcode.cn/problems/jump-game-ii/description/)

```
给定一个长度为 n 的 0 索引整数数组 nums。初始位置为 nums[0]。
每个元素 nums[i] 表示从索引 i 向前跳转的最大长度。换句话说，如果你在 nums[i] 处，你可以跳转到任意 nums[i + j] 处:
1. 0 <= j <= nums[i] 
1. i + j < n
返回到达 nums[n - 1] 的最小跳跃次数。生成的测试用例可以到达 nums[n - 1]。

示例 1:
输入: nums = [2,3,1,1,4]
输出: 2
解释: 跳到最后一个位置的最小跳跃数是 2。
     从下标为 0 跳到下标为 1 的位置，跳 1 步，然后跳 3 步到达数组的最后一个位置。

示例 2:
输入: nums = [2,3,0,1,4]
输出: 2

提示：
1. 1 <= nums.length <= 10^4
2. 0 <= nums[i] <= 1000
3. 题目保证可以到达 nums[n-1]
```

### 解题思路
```
每次找到可到达的最远位置，就可以在线性时间内得到最少的跳跃次数。

在遍历数组时，我们不访问最后一个元素。
因为在访问最后一个元素之前，我们的边界一定大于等于最后一个位置，否则就无法跳到最后一个位置了。
如果访问最后一个元素，在边界正好为最后一个位置的情况下，我们会增加一次「不必要的跳跃次数」，因此我们不必访问最后一个元素。
```

### golang代码
```go
func jump(nums []int) int {
	maxPos := 0  // 当前可到达的最大下标位置
	jumpPos := 0 // 要跳转的位置， 跳转步数+1
	step := 0
	for i := 0; i < len(nums)-1; i++ {
		if maxPos < i+nums[i] {
			maxPos = i + nums[i]
		}
		if i == jumpPos {
			jumpPos = maxPos
			step++
		}
	}

	return step
}
```
