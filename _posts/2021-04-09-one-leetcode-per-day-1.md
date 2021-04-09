---
layout: article
aside:
  toc: true
title:  "每天一题LeetCode之263"
tags: ["LeetCode"]
mathjax: true
---
* content
{:toc}

# 题
```
给你一个整数 n ，请你判断 n 是否为 丑数 。如果是，返回 true ；否则，返回 false 。

丑数 就是只包含质因数 2、3 和/或 5 的正整数。

示例 1：

输入：n = 6
输出：true
解释：6 = 2 × 3

示例 2：

输入：n = 8
输出：true
解释：8 = 2 × 2 × 2

示例 3：

输入：n = 14
输出：false
解释：14 不是丑数，因为它包含了另外一个质因数 7 。

示例 4：

输入：n = 1
输出：true
解释：1 通常被视为丑数。
 
提示：
-2^31 <= n <= 2^31 - 1

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/ugly-number
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

# 解

## 想
最简单的思路:
1. 用规定的质因数集`S`去除该数`m`，得到结果`r`
2. 将`r`赋值给`m`，并重复1的步骤
3. 当`m`等于1时，表明该数是`丑数`
4. 当质因数集`S`无法除尽`m`，且`m`不等于1时，该数不是`丑数`

## 测

**常规用例**

| - | Case 1 | Case 2 | Case 3 | Case 4 | Case 5 | Case 6 |
|--|--|--|--|--|--|--|
| 输入 | 6 | 8 | 10 | 13 | 14 | 300 |
| 输出 | true | true | true | false | false | true |

**特殊用例**

| - | Case 1 | Case 2 | Case 3 | Case 4 |
|--|--|--|--|--|
| 输入 | -2 | -1 | 0 | 1 |
| 输出 | false | false | false | true |


## 码
```JAVA
    public boolean isUgly(int m) {
        if(m <= 0) return false;

        int[] S = new int[]{2, 3, 5};
        while(m != 1) {
            int si = 0;
            while(si < S.length && m % S[si] != 0) si++;

            if(si >= S.length) return m == 1;

            m /= S[si];
        }
        return m == 1;
    }
```

## 析
空间复杂度比较明显，就是 $O(1)$。

时间复杂度上界是 $O(logn)$，质因子至少也是2，极端情况下，如果该数是 $2^n$ 时，达到上界

# 思
leetcode easy题，比较简单。看了题解之后，发现实现上还有更优解法，更容易理解，性能上区别不大，代码行数能降低。方式是每次只用一个质因数去除，直到除不尽为止，当质因数集的所有质因数都除不尽时，判断最终结果是否等于`1`。