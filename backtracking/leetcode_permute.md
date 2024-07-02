# leetcode LCR 083. 全排列

### 题目
[leetcode LCR 083. 全排列](https://leetcode.cn/problems/VvJkup/description/)

```
给定一个不含重复数字的整数数组 nums ，返回其 所有可能的全排列 。可以 按任意顺序 返回答案。

示例 1：
输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]

示例 2：
输入：nums = [0,1]
输出：[[0,1],[1,0]]

示例 3：
输入：nums = [1]
输出：[[1]]
 

提示：
1. 1 <= nums.length <= 6
2. -10 <= nums[i] <= 10
3. nums 中的所有整数 互不相同
```

### 解题思路
```
newNums数组表示存储结果
第一次for循环，newNums[0] 的情况有 len(nums)个。used[0]表示newNums[0]的存储情况
对于后面第n个数据，for循环nums，判断数据在used中是否位true
    1. true，表示数据在newNums的前n-1个数据中已经存在，直接pass
    2. false，newNums[n] = nums[i]

递归实现
```
[B站解题思路讲解](https://www.bilibili.com/video/BV19v4y1S79W/?spm_id_from=333.788&vd_source=73dd03c0bb666ba0c69b2ea7fee8b249)
[B站解题思路对应文档](https://programmercarl.com/0046.%E5%85%A8%E6%8E%92%E5%88%97.html)


### golang代码
```go
func permute(nums []int) [][]int {
	sort.Ints(nums)

	res := make([][]int, 0)           // 存储结果
	used := make([]bool, len(nums))   // 数据在之前是否被用过，key 采用下标
	path := make([]int, 0, len(nums)) // 每种情况的存储

	var bt func(int)
	bt = func(index int) {
		if index == len(nums) { // 到底了
			tmp := make([]int, len(path))
			copy(tmp, path)
			res = append(res, tmp)
		}

		for i := 0; i < len(nums); i++ {
			if !used[i] { // 表示数据在之前的层级中被使用了
				path = append(path, nums[i])
				used[i] = true
				bt(index + 1)
				used[i] = false
				path = path[:len(path)-1]
			}
		}
	}

	bt(0)
	return res
}
```
