# 单调栈：移掉 K 位数字

#### [leetcode题目-402. 移掉 K 位数字](https://leetcode.cn/problems/remove-k-digits/description/)
```
给你一个以字符串表示的非负整数 num 和一个整数 k ，移除这个数中的 k 位数字，使得剩下的数字最小。请你以字符串形式返回这个最小的数字。
 
示例 1 ：
输入：num = "1432219", k = 3
输出："1219"
解释：移除掉三个数字 4, 3, 和 2 形成一个新的最小的数字 1219 。
示例 2 ：

输入：num = "10200", k = 1
输出："200"
解释：移掉首位的 1 剩下的数字为 200. 注意输出不能有任何前导零。
示例 3 ：

输入：num = "10", k = 2
输出："0"
解释：从原数字移除所有的数字，剩余为空就是 0 。
 

提示：
1. 1 <= k <= num.length <= 10^5
2. num 仅由若干位数字（0 - 9）组成
3. 除了 0 本身之外，num 不含任何前导零
```

#### 解题思路-单调栈
在保持原有序列不变的情况下，去除k位数字，使数据最小。

对于数据abxxxx，去掉 a 或者 b 其中一位数据，肯定去掉其中比较大的值，可以使数据最小。

```
如何保证数据最小呢，要尽量保证，去除k位数据后， num[i] < num[i+1] 
```
具体流程如下：
```
1. 遍历num，将数据按照单调递增的方式入栈，如果不满足num[i] < num[i+1],则去除num[i+1]，并将k-1
    a. 当k=0，不再需要移除数据，直接将后面剩余的部分入栈即可。
2. 遍历完num后， 此时栈内数据单调递增，满足任意i，num[i] < num[i+1]
    a. 如果k > 0 , 去除栈内最后k位数据即可。
```


##### golang实现
```go
func removeKdigits(num string, k int) string {
	stack := make([]byte, len(num)) // 栈内单调递增
	stackLen := 0

	for i := 0; i < len(num); i++ {
		curNum := num[i]
		for j := stackLen - 1; j >= 0 && k > 0; j-- { // 用当前数据以此和栈内数据做对比，直到所有大于当前数据的元素被清除（k  > 0 的情况下）
			if stack[j] > curNum && k > 0 { // 栈内数据 大于 当前数据，移除
				k--
				stackLen--
			} else {
				break
			}
		}

		stack[stackLen] = curNum
		stackLen++
	}

	res := string(stack[:stackLen])
	res = res[:stackLen-k]           // 如果k  > 0,此时栈内数据单调递增，只需要去除最后面的k位数据即可
	res = strings.TrimLeft(res, "0") // 将最前面的0去除
	if res == "" {
		res = "0" // 为空返0
	}

	return res
}
```