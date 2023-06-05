---
hide:
  - toc
title: Day 33 动态规划
---

[416. 分割等和子集](https://leetcode.cn/problems/partition-equal-subset-sum/)

**题目**：给定一个只包含正整数的非空数组。是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。

注意: 每个数组中的元素不会超过 100 数组的大小不会超过 200

```cpp
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int sum = accumulate(nums.begin(), nums.end(), 0);
        if (sum % 2 != 0) {
            return false;
        }
        sum /= 2;
        vector<vector<bool>>dp(sum + 1, vector<bool>(nums.size() + 1, false));
        dp[0][0] = true;
        for (int i = 1; i <= sum; i++) {
            for (int j = 1; j <= nums.size(); j++) {
                if (i - nums[j - 1] >= 0) {
                    dp[i][j] = dp[i][j - 1] || dp[i - nums[j - 1]][j - 1];
                } else {
                    dp[i][j] = dp[i][j - 1];
                }
            }
        }
        return dp[sum][nums.size()];

    }
};
```

 