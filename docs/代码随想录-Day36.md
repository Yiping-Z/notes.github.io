---
hide:
  - toc
title: Day 36 动态规划
---

### [96. 不同的二叉搜索树](https://leetcode.cn/problems/unique-binary-search-trees/)

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

### [416. 分割等和子集](https://leetcode.cn/problems/partition-equal-subset-sum/)

**题目**： 给你一个 只包含正整数 的 非空 数组 nums 。请你判断是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。

**思路**： 01背包，先遍历物品再遍历背包容量。

```cpp
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int sum = accumulate(nums.begin(), nums.end(), 0);
        if (sum % 2 != 0) {
            return false;
        }
        sum /= 2;
        vector<bool>dp(sum + 1, false);
        dp[0] = true;
        for (int i = 0; i < nums.size(); i++) {
            for (int j = sum; j >= nums[i]; j--) {
                dp[j] = dp[j] || dp[j - nums[i]];
            }
        }
        return dp[sum];
    }
};
```