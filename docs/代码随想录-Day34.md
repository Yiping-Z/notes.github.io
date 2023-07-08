---
hide:
  - toc
title: Day 34 动态规划
---
### 01背包

![在这里插入图片描述](https://img-blog.csdnimg.cn/6f55926d33894fe5a28b43f155616f6e.png)


**初始化**: 一定要和dp数组的定义吻合，否则到递推公式的时候就会越来越乱。

```cpp
// 初始化 dp
vector<vector<int>> dp(weight.size(), vector<int>(bagweight + 1, 0));
for (int j = weight[0]; j <= bagweight; j++) {
    dp[0][j] = value[0];
}
```

**递推公式**: dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]);

确定**遍历顺序**

二维dp遍历的时候，背包容量是从小到大，而一维dp遍历的时候，背包是从大到小。
倒序遍历是为了保证物品i只被放入一次

二维dp数组历的时候不用倒序: 因为对于二维dp，dp[i][j]都是通过上一层即dp[i - 1][j]计算而来，本层的dp[i][j]并不会被覆盖

两个嵌套for循环的顺序，代码中是先遍历物品嵌套遍历背包容量，不可以先遍历背包容量嵌套遍历物品

如果遍历背包容量放在上一层，那么每个dp[j]就只会放入一个物品

倒序遍历的原因是，本质上还是一个对二维数组的遍历，并且右下角的值依赖上一层左上角的值，因此需要保证左边的值仍然是上一层的，从右向左覆盖。

```cpp
// weight数组的大小 就是物品个数
for(int j = 0; j <= bagweight; j++) { // 遍历背包容量
    for(int i = 1; i < weight.size(); i++) { // 遍历物品
        if (j < weight[i]) dp[i][j] = dp[i - 1][j];
        else dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]);
    }
}

// 一维滚动数组
vector<int> dp(bagWeight + 1, 0);
for(int i = 0; i < weight.size(); i++) { // 遍历物品
    for(int j = bagWeight; j >= weight[i]; j--) { // 遍历背包容量
        dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
    }
}
```

在求装满背包有几种方法的情况下，递推公式一般为：
```cpp
dp[j] += dp[j - nums[i]];
```

### 完全背包
完全背包的物品是可以添加多次的 <br>
对于纯完全背包问题，其for循环的先后循环是可以颠倒的

```cpp
// 先遍历物品，再遍历背包
for(int i = 0; i < weight.size(); i++) { // 遍历物品
    for(int j = weight[i]; j <= bagWeight ; j++) { // 遍历背包容量
        dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);

    }
}

// 组合数
for (int i = 0; i < coins.size(); i++) { // 遍历物品
    for (int j = coins[i]; j <= amount; j++) { // 遍历背包容量
        dp[j] += dp[j - coins[i]];
    }
}

// 排列数， [1, 5], [5, 1]是两组数
for (int j = 0; j <= amount; j++) { // 遍历背包容量
    for (int i = 0; i < coins.size(); i++) { // 遍历物品
        if (j - coins[i] >= 0) dp[j] += dp[j - coins[i]];
    }
}
```

### [1049. 最后一块石头的重量 II](https://leetcode.cn/problems/last-stone-weight-ii/)

**题目**：有一堆石头，用整数数组 stones 表示。其中 stones[i] 表示第 i 块石头的重量。

每一回合，从中选出任意两块石头，然后将它们一起粉碎。假设石头的重量分别为 x 和 y，且 x <= y。那么粉碎的可能结果如下：

如果 x == y，那么两块石头都会被完全粉碎；
如果 x != y，那么重量为 x 的石头将会完全粉碎，而重量为 y 的石头新重量为 y-x。
最后，最多只会剩下一块 石头。返回此石头 最小的可能重量 。如果没有石头剩下，就返回 0。

**思路**：尽量让石头分成重量相同的两堆，相撞之后剩下的石头最小<br>
dp[j]表示容量（这里说容量更形象，其实就是重量）为j的背包，最多可以背最大重量为dp[j]。<br>
物品遍历的for循环放在外层，遍历背包的for循环放在内层，且内层for循环倒序遍历

```cpp
class Solution {
public:
    int lastStoneWeightII(vector<int>& stones) {
        vector<int> dp(15001, 0);
        int sum = accumulate(stones.begin(), stones.end(), 0);
        int target = sum / 2;
        for (int i = 0; i < stones.size(); i++) { // 遍历物品
            for (int j = target; j >= stones[i]; j--) { // 遍历背包
                dp[j] = max(dp[j], dp[j - stones[i]] + stones[i]);
            }
        }
        return sum - dp[target] - dp[target];
    }
};
```

###[494.目标和](https://leetcode.cn/problems/target-sum/)

**题目**：给定一个非负整数数组，a1, a2, ..., an, 和一个目标数，S。现在你有两个符号 + 和 -。对于数组中的任意一个整数，你都可以从 + 或 -中选择一个符号添加在前面。

返回可以使最终数组和为目标数 S 的所有添加符号的方法数。

**思路**：
回溯算法：
left组合 - right组合 = target。

left + right = sum，而sum是固定的。right = sum - left

left - (sum - left) = target 推导出 left = (target + sum)/2 。

问题就是在集合nums中找出和为left的组合。

动态规划：
// 要求背包装满
dp[j] += dp[j - nums[i]];

```cpp
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int target) {
        int sum = accumulate(nums.begin(), nums.end(), 0);
        if (abs(target) > sum) {
            return 0;
        }
        if ((sum + target) % 2 != 0) {
            return 0;
        }
        int v = (target + sum) / 2;
        vector<int>dp(v + 1, 0);
        dp[0] = 1;
        for (int i = 0; i < nums.size(); i++) {
            for (int j = v; j >= nums[i]; j--) {
                dp[j] += dp[j - nums[i]];
            }
        }
        return dp[v];
    }
};
```
###[474.一和零](https://leetcode.cn/problems/ones-and-zeroes/)

**题目**：给你一个二进制字符串数组 strs 和两个整数 m 和 n 。

请你找出并返回 strs 的最大子集的大小，该子集中 最多 有 m 个 0 和 n 个 1 。

如果 x 的所有元素也是 y 的元素，集合 x 是集合 y 的 子集 。

```cpp
class Solution {
public:
    int findMaxForm(vector<string>& strs, int m, int n) {
        vector<vector<int>>dp(m + 1, vector<int>(n + 1));
        for (string str: strs) {
            int cnt0 = 0;
            int cnt1 = 0;
            for (auto x: str) {
                if (x == '1') {
                    cnt1++;
                } else {
                    cnt0++;
                }
            }
            for (int i = m; i >= cnt0; i--) {
                for (int j = n; j >= cnt1; j--) {
                    dp[i][j] = max(dp[i][j], dp[i - cnt0][j - cnt1] + 1);
                }
            }
        }
        return dp[m][n];
    }
};
```
