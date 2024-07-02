# 跳跃游戏 III

### 题目
[leetcode 1306. 跳跃游戏 III](https://leetcode.cn/problems/jump-game-iii/)

```
这里有一个非负整数数组 arr，你最开始位于该数组的起始下标 start 处。当你位于下标 i 处时，你可以跳到 i + arr[i] 或者 i - arr[i]。
请你判断自己是否能够跳到对应元素值为 0 的 任一 下标处。
注意，不管是什么情况下，你都无法跳到数组之外。

示例 1:
输入：arr = [4,2,3,0,3,1,2], start = 5
输出：true
解释：
    到达值为 0 的下标 3 有以下可能方案： 
    下标 5 -> 下标 4 -> 下标 1 -> 下标 3 
    下标 5 -> 下标 6 -> 下标 4 -> 下标 1 -> 下标 3 

示例 2：
输入：arr = [4,2,3,0,3,1,2], start = 0
输出：true 
解释：
    到达值为 0 的下标 3 有以下可能方案： 
    下标 0 -> 下标 4 -> 下标 1 -> 下标 3

示例 3：
输入：arr = [3,0,2,1,2], start = 2
输出：false
解释：无法到达值为 0 的下标 1 处。 

提示：
1. 1 <= arr.length <= 5 * 10^4
2. 0 <= arr[i] < arr.length
3. 0 <= start < arr.length
```

### 解题思路
```
正向查找，回溯思想
正向，从start下标开始，以此查询其走的路径，如果存在 数据为0，则返回true，如果走到了之前走过的点，则为false，回到上一层级
```

### golang代码
```go
func canReach(arr []int, start int) bool {
	pathed := make([]bool, len(arr))
	res := false
	var dt func(int)
	dt = func(index int) {
		nextIndexs := make([]int, 0, 2)
		if index+arr[index] < len(arr) {
			nextIndexs = append(nextIndexs, index+arr[index])
		}
		if index-arr[index] >= 0 && arr[index] != 0 {
			nextIndexs = append(nextIndexs, index-arr[index])
		}

		for i := 0; i < len(nextIndexs); i++ {
			if arr[nextIndexs[i]] == 0 {
				res = true
				return
			}

			if !pathed[nextIndexs[i]] {
				pathed[nextIndexs[i]] = true
				dt(nextIndexs[i])
				pathed[nextIndexs[i]] = false
			}
		}
	}

	dt(start)
	return res
}
```
