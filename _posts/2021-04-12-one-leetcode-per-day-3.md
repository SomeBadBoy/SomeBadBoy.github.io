---
layout: article
aside:
  toc: true
title:  "每天一题LeetCode之179"
tags: ["LeetCode"]
mathjax: true
---
* content
{:toc}

# 题
```
给定一组非负整数 nums，重新排列每个数的顺序（每个数不可拆分）使之组成一个最大的整数。

注意：输出结果可能非常大，所以你需要返回一个字符串而不是整数。

示例 1：

输入：nums = [10,2]
输出："210"

示例 2：

输入：nums = [3,30,34,5,9]
输出："9534330"

示例 3：

输入：nums = [1]
输出："1"

示例 4：

输入：nums = [10]
输出："10"
 
提示：

1 <= nums.length <= 100
0 <= nums[i] <= 10^9

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/largest-number
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

# 解

## 想
考虑这样一个数组`[99, 98, 97, 9, 8]`，能组成的最大的数是`99998978`，也就是`9`或`99`放在第一位和第二位，`98`放第三位，`97`放第四位，`8`放最后。将单纯的由`9`组合的数字放在最高位，越靠近`1`的放低位，这样才能得到一个比较大的数字。

考虑`886`和`88`，哪个应该放高位呢？应该是`88`。那么`889`和`88`呢？应该是`889`。也就是当两个数字长度不同时，长度为$a$和$b$，对于更长的那个数字的$a-b$位，如果其大于第一位，则应当用更长的数字，否则用更短的；如果相同，也应当用更短的。

对于任意两个数字$a$和$b$，假设$l_a$和$l_b$分别是$a$和$b$的长度（也就是数字的个数），$a_1$和$b_1$分别是数字$a$和数字$b$的最高位数字，$a_n$和$b_n$表示后续的第$n$位数字。那么在以下情况时，$a$摆放位置比$b$要靠前：
1. 当$l_a=l_b$时
    - $a_0=b_0,a_1=b_1...a_n=b_n$且$a_{n+1} > b_{n+1}$
2. 当$l_a>l_b$时
    - $a_0=b_0,a_1=b_1...a_n=b_n$且$a_{n+1}>b_0$
    - $a_0=b_0,a_1=b_1...a_n=b_n$且$a_{n+1}=b_0,a_{n+2}=b_0...a_{n+m}=b_0$且$a_{n+m+1}>b_0$
3. 当$l_a<l_b$时，与2的情况相反

这里可以利用数字首位，将所有数字分到十个桶中，对每个桶里的数字，按照上面的规则进行排序，从第9个桶顺序由大到小排列，得到最终的结果。考虑到题目中给的数字个数较少，最多100个的情况下，可以简单一点，不用分桶，将所有数字转成字符串后直接排序。

## 测
1. [99, 998, 88, 988, 889]
2. [10, 2, 0]
3. [11, 112, 110, 111]
4. [432, 43243] (第一次提交后补充)
5. [0, 0]

## 码
```JAVA
    public String largestNumber(int[] nums) {
        List<String> src = new ArrayList<>();
        boolean allZero = true;
        for(int i = 0; i < nums.length; i++) {
            allZero = allZero && nums[i] == 0;
            src.add(String.valueOf(nums[i]));
        }
        if(allZero) return "0";
        
        src.sort((o1, o2) -> compare(o1, o2));
        StringBuilder builder = new StringBuilder();
        for(int i = src.size() - 1; i >= 0; i--) {
            builder.append(src.get(i));
        }
        return builder.toString();
    }
    
    public int compare(String a, String b) {
        for(int i = 0; i < 2* Math.max(a.length(), b.length()); i++) {
            int ia = i % a.length(), ib = i % b.length();
            if(a.charAt(ia) > b.charAt(ib)) return 1;
            else if(a.charAt(ia) < b.charAt(ib)) return -1;
        }
        return a.length() < b.length() ? 1 : -1;
    }

```

## 析
时间复杂度：数组中数字最大为$10^9$，因此最多比较18次，排序使用的是JAVA自带的排序，时间复杂度是$O(N \log N)$，因此最终的时间复杂度是$O(N \log N)$。

空间复杂度是$O(1)$

# 思
官方题解思路比较严谨，证明了排序的充分性和必要性，整个思路非常值得学习。根据经验也许可以得到正确的结论，但是严谨的思维方式和推导过程更有助于发现规律，更具有一般性。

这题卡了快三天，在思路上其实还有欠缺，实现上没有找到特别好的方式。