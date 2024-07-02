# 无重复字符的最长子串

### 题目
[leetcode 3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)

```
给定一个字符串 s ，请你找出其中不含有重复字符的 最长子串 的长度。

示例 1:
输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

示例 2:
输入: s = "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。

示例 3:
输入: s = "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。

提示：
1. 0 <= s.length <= 5 * 104
2. s 由英文字母、数字、符号和空格组成
```

### 解题思路
```
初始化定义：
1. 定义i、j两个指针，从头i=0，j=1的位置判断，
2. strmap[str]int记录每个字符出现位置的最大下标值。
3. max表示无重复字符的最长子串 长度
for循环字符串中的每一个字符，比如读取到字符str（j所指向的位置）：
    1. 如果str在strmap不存在，则表示i到j之间的字符不重复，max=(j-i+1,max)；并记录strmap[str]=j,j++
    2. 如果str在strmap存在，    
        a. strmap[str] < i , 表示str在i和j之间无重复字符，max=(j-i+1,max);strmap[str]=j；j++
        b. strmap[str] >= i, 表示出现了重复字符串，i=strmap[str]+1;strmap[str]=j
    3. 直到循环结束
```

### go代码
```go
func lengthOfLongestSubstring(s string) int {
	lens := len(s) // 边界线
	if lens <= 1 {
		return lens
	}

	i, j, max := 0, 1, 1
	m := make(map[string]int) // map[字母]字母所在位置
	m[string(s[0])] = 0

	for j < lens && i < j {
		t, ok := m[string(s[j])]
		if ok && t+1 > i {
			i = t + 1
		}
		m[string(s[j])] = j
		j++
		mt := j - i
		if mt > max {
			max = mt
		}

	}

	return max
}
```