# 单调栈：不同字符的最小子序列


#### leetcode题目
1. [leetcode题目-1081. 不同字符的最小子序列](https://leetcode.cn/problems/smallest-subsequence-of-distinct-characters/description/)
    返回 s 字典序最小的子序列，该子序列包含 s 的所有不同字符，且只包含一次。
2. [leetcode题目-316. 去除重复字母](https://leetcode.cn/problems/remove-duplicate-letters/description/)
    给你一个字符串 s ，请你去除字符串中重复的字母，使得每个字母只出现一次。需保证 返回结果的字典序最小（要求不能打乱其他字符的相对位置）。

以上两个题目完全一样

#### 解题思路
和前面的移除K个字母一样，但是不同的是，每个字母在栈中只出现一次。
```
定义inStack map记录字母在栈中是否存在
```
当s[i]<s[i+1],s[i+1]是否可被遗弃，要取决于之后该数据还是否存在，如果不存在，则不遗弃。
```
用letterCount map记录每个字母的数量，每次遍历到，都 -1
```

#### golang实现
```go
func smallestSubsequence(s string) string {
	strLen := len(s)
	stack := make([]byte, 0)
	inStack := make(map[byte]bool)

	letterCount := make(map[byte]int) // 计算每个字母的数量
	for i := 0; i < strLen; i++ {
		letterCount[s[i]]++
	}

	for i := 0; i < strLen; i++ {
		cur := s[i]
        letterCount[cur]--
		if in, _ := inStack[cur]; in {
			continue // 如果数据已经在栈内存在，pass
		}

		for len(stack) > 0 {
			pre := stack[len(stack)-1]
			if cur < pre && letterCount[pre] > 0 {
				inStack[pre] = false
				stack = stack[:len(stack)-1]
			} else {
				break
			}
		}

		stack = append(stack, cur)
		inStack[cur] = true
	}

	return string(stack)
}

```