---
hide:
  - toc
title: Day 23 回溯
---
[39. Combination Sum](https://leetcode.cn/problems/combination-sum/)

题目：
Given an array of distinct integers candidates and a target integer target, return a list of all unique combinations of candidates where the chosen numbers sum to target. You may return the combinations in any order.

The same number may be chosen from candidates an unlimited number of times. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

The test cases are generated such that the number of unique combinations that sum up to target is less than 150 combinations for the given input.

```cpp
class Solution {
public:
    vector<vector<int>>res;
    void permutate(vector<int>&nums, int sum, int idx, int target, vector<int>num) {
        if (sum >= target) {
            if (sum == target) {
                res.push_back(num);
            }
            return;
        }
        // 剪枝
        for (int i = idx; i < nums.size() && sum + nums[i] <= target; i++) {
            num.push_back(nums[i]);
            permutate(nums, sum + nums[i], i, target, num);
            num.pop_back();
        }
    }
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<int>num;
        // 需要排序
        sort(candidates.begin(), candidates.end()); 
        permutate(candidates, 0, 0, target, num);
        return res;
    }
};
```
[40. Combination Sum II](https://leetcode.cn/problems/combination-sum-ii/)
