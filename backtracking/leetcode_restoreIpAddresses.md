# leetcode 93 复原 IP 地址

### 题目
[leetcode 93 复原 IP 地址](https://leetcode.cn/problems/restore-ip-addresses/description/)

```
有效 IP 地址 正好由四个整数（每个整数位于 0 到 255 之间组成，且不能含有前导 0），整数之间用 '.' 分隔。
例如：
1. "0.1.2.201" 和 "192.168.1.1" 是 有效 IP 地址.
2. "0.011.255.245"、"192.168.1.312" 和 "192.168@1.1" 是 无效 IP 地址。
给定一个只包含数字的字符串 s ，用以表示一个 IP 地址，返回所有可能的有效 IP 地址，这些地址可以通过在 s 中插入 '.' 来形成。
你 不能 重新排序或删除 s 中的任何数字。你可以按 任何 顺序返回答案。

示例 1：
输入：s = "25525511135"
输出：["255.255.11.135","255.255.111.35"]

示例 2：
输入：s = "0000"
输出：["0.0.0.0"]

示例 3：
输入：s = "101023"
输出：["1.0.10.23","1.0.102.3","10.1.0.23","10.10.2.3","101.0.2.3"]
 

提示：
1. 1 <= s.length <= 20
2. s 仅由数字组成
```

### 解题思路
```
IP = addr1.addr2.addr3.addr4 = s(提供的字符串)。
addr1，addr2，addr3 分别以此取1、2、3位数据，剩余的数据位addr4.
判断每一个取到的addr是否满足条件
    1. 如果全部满足条件，则放入res中
    2. 如果有一个不满足条件，退回到上一阶段。
```

### golang代码
```go
func restoreIpAddresses(s string) []string {
	address := make([]string, 4) // 存储枚举地址
	res := make([]string, 0)     // 存储最后符合条件的结果
	res = singleAddr(s, address, res, 1)
	fmt.Println(res)
	return res
}

/*
s：待截取前x位的字符串
address：用来存储枚举地址的字段
res：存储最后的结果
l：记录 计算到第几位 ip地址了。

IP由4位地址组成，1<=l<=4。
*/
func singleAddr(s string, address, res []string, l int) []string {
	if len(s) == 0 {
		return res
	}

	if l == 4 {
		if isIPAddr(s) {
			address[l-1] = s
		} else {
			return res
		}
		res = append(res, strings.Join(address, "."))
	} else {
		minLen := 3
		if len(s) < minLen {
			minLen = len(s)
		}
		for i := 1; i < minLen+1; i++ { // 以此取1、2、3位字符
			sub := s[:i]
			if isIPAddr(sub) {
				address[l-1] = sub
			} else {
				continue
			}
			res = singleAddr(s[i:], address, res, l+1)
		}
	}
	return res
}

// 判断单个addr是否满足条件
func isIPAddr(addr string) bool {
	if addr == "" { // 为空，不满足
		return false
	}
	if string(addr[0]) == "0" && len(addr) > 1 { // 含有前导 0，不满足
		return false
	}

	subNum, _ := strconv.Atoi(addr)
	return subNum <= 255 // 数据大于255，不满足
}
```
