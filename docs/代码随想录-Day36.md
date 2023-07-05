---
hide:
  - toc
title: Day 36 动态规划
---

[96. 不同的二叉搜索树](https://leetcode.cn/problems/unique-binary-search-trees/)

**题目**：给你一个整数 n ，求恰由 n 个节点组成且节点值从 1 到 n 互不相同的 二叉搜索树 有多少种？返回满足题意的二叉搜索树的种数。

**思路**：dp[i] += dp[以j为头结点左子树节点数量] * dp[以j为头结点右子树节点数量]


```cpp
class Solution {
public:
    int numTrees(int n) {
        vector<int>dp(n + 1);
        dp[0] = 1;
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= i; j++) {
                dp[i] += dp[j - 1] * dp[i - j];
            } 
        }
        return dp[n];
    }
};
```