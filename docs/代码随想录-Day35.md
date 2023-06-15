---
hide:
  - toc
title: Day 35 动态规划
---
0-1背包的多种应用，

纯 0 - 1 背包 (opens new window)是求 给定背包容量 装满背包 的最大价值是多少。<br>
416. 分割等和子集 (opens new window)是求 给定背包容量，能不能装满这个背包。<br>
1049. 最后一块石头的重量 II (opens new window)是求 给定背包容量，尽可能装，最多能装多少<br>
494. 目标和 (opens new window)是求 给定背包容量，装满背包有多少种方法。<br>
474 是求 给定背包容量，装满背包最多有多少个物品。

完全背包的物品是可以添加多次的，所以要从小到大去遍历。
// 先遍历物品，再遍历背包

```cpp
for(int i = 0; i < weight.size(); i++) { // 遍历物品
    for(int j = weight[i]; j <= bagWeight ; j++) { // 遍历背包容量
        dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);

    }
}
```
###[518.零钱兑换II](https://leetcode.cn/problems/coin-change-ii/)

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
