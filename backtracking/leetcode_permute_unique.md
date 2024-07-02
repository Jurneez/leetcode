# leetcode LCR 084. 全排列 II

### 题目
[leetcode LCR 084. 全排列 II](https://leetcode.cn/problems/7p8L0Z/)

```
给定一个可包含重复数字的序列 nums ，按任意顺序 返回所有不重复的全排列。

示例 1：
输入：nums = [1,1,2]
输出：
[[1,1,2],
 [1,2,1],
 [2,1,1]]

示例 2：
输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]

提示：
1. 1 <= nums.length <= 8
2. -10 <= nums[i] <= 10
```

### 解题思路
```
nums数据有重复，所以需要通过对数组进行排序，方便后续同层去重复。

newNums数组表示存储结果
第一次for循环，newNums[0] 的情况有 len(nums)个。used[0]表示newNums[0]的使用状态
对于后面第n个数据，for循环nums，判断数据在used中的状态
    1. true，表示数据在newNums的前n-1个数据中已经存在，直接pass
    2. false，判断当前数据和前面的数据是否一致
        a. 一致，表示在同层中数据已经使用过，不可重复使用
        b. 不一致，直接使用

递归实现
```
[B站解题思路讲解](https://www.bilibili.com/video/BV1R84y1i7Tm/?spm_id_from=333.788&vd_source=73dd03c0bb666ba0c69b2ea7fee8b249)
[B站解题思路对应文档](https://programmercarl.com/0047.%E5%85%A8%E6%8E%92%E5%88%97II.html)

### golang代码
```go
func permuteUnique(nums []int) [][]int {
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
			if i > 0 && nums[i-1] == nums[i] && !used[i-1] { // 表示相同的数据在本轮被使用了
				continue
			}
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
