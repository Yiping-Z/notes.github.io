---
hide:
  - toc
title: Day 35 动态规划
---

### [518.零钱兑换II](https://leetcode.cn/problems/coin-change-ii/)

**题目**：给定不同面额的硬币和一个总金额。写出函数来计算可以凑成总金额的硬币组合数。假设每一种面额的硬币有无限个。

**思路**： 如果求组合数就是外层for循环遍历物品，内层for遍历背包。
如果求排列数就是外层for遍历背包，内层for循环遍历物品。

```cpp
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        vector<int>dp(amount + 1, 0);
        dp[0] = 1;
        for (int j = 0; j < coins.size(); j++) { // 遍历物品 只会出现[1, 5], 不会出现[5, 1]
            for (int i = 1; i <= amount; i++) { // 遍历背包
                if (i - coins[j] >= 0) {
                    dp[i] += dp[i - coins[j]];
                }
            }
        }
        return dp[amount];
    }
};
```
