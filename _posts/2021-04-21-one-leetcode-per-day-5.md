---
layout: article
aside:
  toc: true
title:  "每天一题LeetCode之91"
tags: ["LeetCode"]
mathjax: true
---
* content
{:toc}

# 题
```
一条包含字母 A-Z 的消息通过以下映射进行了 编码 ：

'A' -> 1
'B' -> 2
...
'Z' -> 26
要 解码 已编码的消息，所有数字必须基于上述映射的方法，反向映射回字母（可能有多种方法）。例如，"11106" 可以映射为：

"AAJF" ，将消息分组为 (1 1 10 6)
"KJF" ，将消息分组为 (11 10 6)
注意，消息不能分组为  (1 11 06) ，因为 "06" 不能映射为 "F" ，这是由于 "6" 和 "06" 在映射中并不等价。

给你一个只含数字的 非空 字符串 s ，请计算并返回 解码 方法的 总数 。

题目数据保证答案肯定是一个 32 位 的整数。

示例 1：

输入：s = "12"
输出：2
解释：它可以解码为 "AB"（1 2）或者 "L"（12）。

示例 2：

输入：s = "226"
输出：3
解释：它可以解码为 "BZ" (2 26), "VF" (22 6), 或者 "BBF" (2 2 6) 。

示例 3：

输入：s = "0"
输出：0
解释：没有字符映射到以 0 开头的数字。
含有 0 的有效映射是 'J' -> "10" 和 'T'-> "20" 。
由于没有字符，因此没有有效的方法对此进行解码，因为所有数字都需要映射。

示例 4：

输入：s = "06"
输出：0
解释："06" 不能映射到 "F" ，因为字符串含有前导 0（"6" 和 "06" 在映射中并不等价）。
 
提示：

1 <= s.length <= 100
s 只包含数字，并且可能包含前导零。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/decode-ways
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

# 解

## 想
第一想法就是利用动态规划来解，跟上台阶问题差不多。
$$f(n) = a_1f(n-1) + a_1a_2f(n-2)$$
其中$a_1f(n-1)$和$a_1a_2f(n-2)$都可能为空，那么利用动态规划求解，可以使用`Map<Integer, Integer>`，key作为起始位置，value为当前起始位置的解析数量。由整个字符串的末尾往前遍历，最终得到需要求解的结果。

## 测
1. 12
2. 10
3. 120
4. 30

## 码
```JAVA
    public int numDecodings(String s) {
        if(s == null || s.equals("") || s.charAt(0) == '0') return 0;

        char[] str = s.toCharArray();
        Map<Integer, Integer> map = new HashMap<>();

        int l = str.length;
        // 这里是给状态转移方程中的最初两个状态进行计算赋值
        if(str[l - 1] != '0') map.put(l - 1, 1);
        if(l > 1) {
            if(str[l - 2] == '0') map.put(l - 2, 0);
            else if(canDecode(str[l - 2], str[l - 1])) {
                map.put(l - 2, map.getOrDefault(l - 1, 0) + 1);
            } else if (str[l - 1] != '0'){
                map.put(l - 2, 1);
            }
        }
        
        // 根据状态转移方程进行计算
        for(int i = l - 3; i >= 0; i--) {
            int p = map.getOrDefault(i + 1, 0);
            int pp = map.getOrDefault(i + 2, 0);
            if(str[i] == '0') {
                map.put(i, 0);
                continue;
            }
            if(canDecode(str[i], str[i + 1])) {
                map.put(i, p + pp);
            } else {
                map.put(i, p);
            }
        }
        return map.getOrDefault(0, 0);
    }

    public boolean canDecode(char a, char b) {
        if(a == '1') return true;
        if(a == '2') return b == '0' || b <= '6';
        return false;
    }
```

## 析
时间复杂度$O(n)$。空间复杂度$O(n)$。

# 思
题解思路也是用动态规划方式处理，不同的是官方题解从第一位开始逐步往后计算，这样可能更容易理解，编码上也会简洁一点。另外由于只使用了$f(n-1)$和$f(n-2)$两个状态，所以可以把Map进一步缩减为两个变量，这样空间复杂度将降为$O(1)$.

上一道题卡了很久，今天决定先写下一道，上一道题就先留着再完善。