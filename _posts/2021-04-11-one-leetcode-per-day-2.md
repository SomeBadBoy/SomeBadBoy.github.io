---
layout: article
aside:
  toc: true
title:  "每天一题LeetCode之501"
tags: ["LeetCode"]
mathjax: true
---
* content
{:toc}

# 题
```
给定一个有相同值的二叉搜索树（BST），找出 BST 中的所有众数（出现频率最高的元素）。

假定 BST 有如下定义：

结点左子树中所含结点的值小于等于当前结点的值
结点右子树中所含结点的值大于等于当前结点的值
左子树和右子树都是二叉搜索树
例如：
给定 BST [1,null,2,2],

   1
    \
     2
    /
   2
返回[2].

提示：如果众数超过1个，不需考虑输出顺序

进阶：你可以不使用额外的空间吗？（假设由递归产生的隐式调用栈的开销不被计算在内）

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/find-mode-in-binary-search-tree
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

# 解

## 想
考虑到BST的特性，采用中序遍历，就能得到一个有序数列，因此可以转化为在一个有序数组中，找到所有的众数。

而在一个有序数组中，找到所有的众数，有两个思路
1. 使用一个map，以数值为key，出现的次数为value，遍历整个数组，得到所有数的计数次数。最终遍历这个map，得到value最大的数，并输出数值。该方式时间复杂度是 $O(n)$，需遍历两次。
2. 对于题目中要求的不使用额外空间的话，可以先遍历一遍，找到众数的出现次数，再遍历第二遍，对出现次数等于众数出现次数的数进行输出。

## 测
树的测试用例不容易表示，这里就简单描述下
1. 构造众数出现次数>1的树，且只有一个众数
2. 构造众数出现次数>1的树，且有多个众数
3. 构造众数出现次数=1的树，有多个众数
4. 构造众数出现次数=1的树，且只有一个众数（也就是只有一个节点）
5. 构造一个空的树

## 码
```JAVA
    public int[] findMode(TreeNode root) {
        if(root.left == null && root.right == null) return new int[]{root.val};
        
        Counter counter = new Counter();
        visit(root, counter);
        if(counter.maxValueCount == 1) {
            return new int[]{counter.maxValue};
        }
        Printer printer = new Printer(counter.maxCount, counter.maxValueCount);
        visit(root, printer);
        return printer.meets;
    }

    public void visit(TreeNode tree, Reporter reporter) {
        visitChildren(tree, reporter);
        reporter.afterAll();
    }

    public void visitChildren(TreeNode tree, Reporter reporter) {
        if(tree == null) return;
        if(tree.left != null) visitChildren(tree.left, reporter);
        reporter.add(tree.val);
        if(tree.right != null) visitChildren(tree.right, reporter);
    }

    public interface Reporter {
        void add(int value);
        void afterAll();
    }

    public class Printer implements Reporter {
        Integer currentValue;
        int currentCount;
        int[] meets;
        int countValue;
        int meetsIdx;

        public Printer(int maxCount, int size) {
            this.countValue = maxCount;
            this.meets = new int[size];
            this.currentCount = 0;
            this.meetsIdx = 0;
        }

        public void afterAll() {
            if(this.currentCount >= this.countValue) {
                this.meets[this.meetsIdx] = this.currentValue;
            }
        }

        public void add(int value) {
            if(this.currentValue == null || value != this.currentValue) {
                if(this.currentCount >= this.countValue) {
                    this.meets[this.meetsIdx++] = this.currentValue;
                }
                this.currentValue = value;
                this.currentCount = 1;
            } else {
                this.currentCount++;
            }
        }
    }
    public class Counter implements Reporter {
        Integer maxValue;
        int maxCount;
        int maxValueCount;
        Integer currentValue;
        int currentCount;

        public void add(int value) {
            if(currentValue == null || value != currentValue) {
                changeMax();
                this.currentValue = value;
                this.currentCount = 1;
            } else {
                this.currentCount++;
            }
        }

        public void afterAll() {
            changeMax();
        }

        public void changeMax() {
            if(this.currentCount > this.maxCount) {
                this.maxCount = this.currentCount;
                this.maxValue = this.currentValue;
                this.maxValueCount = 1;
            } else if(this.currentCount == this.maxCount) {
                this.maxValueCount++;
            }
        }
    }
```

## 析
空间复杂度是$O(1)$，不过为了达到这个$O(1)$，构造了`Counter`和`Printer`两个类来实现，稍微有点复杂。

时间复杂度是 $O(n)$，一般需要遍历两遍。当众数只有1个时，只需要遍历1遍。

# 思
leetcode easy题，如果不要求空间复杂度的话，应该是比较简单的。要求了空间复杂度之后，显得比较繁琐，多次遍历才能得到最终的结果。

看了官方题解，采用了一个List在第一次遍历过程中就对出现的众数进行保存，当发现当前保存的众数不是真正的众数时，清空该List，也能达到不使用额外空间的目标。这种方式是否是没有使用额外的空间待考虑，因为如果在第一遍查询时就保存目前得到的众数，最坏的情况下，也是一个$O(n)$的空间复杂度。比如众数出现次数等于2，且出现在树的最右侧，那么前面的所有数都会在过程中保存下来。