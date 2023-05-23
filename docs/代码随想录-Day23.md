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

思路：和39题的区别是有重复的数字，但是不可以有重复的位置<br>
同一树层不可以重复，同一树枝可以重复

```cpp
class Solution {
public:
    set<vector<int>>res;
    void permutate(vector<int>&nums, int sum, int idx, int target, vector<int>num) {
        if (sum >= target || idx >= nums.size()) {
            if (sum == target) {
                res.insert(num);
            }
            return;
        }
        for (int i = idx; i < nums.size() && sum + nums[i] <= target; i++) {
        	// 同一个位置不选一样的数字 
            if (i > idx && nums[i] == nums[i - 1]) {
                continue;
            }
            num.push_back(nums[i]);
            permutate(nums, sum + nums[i], i + 1, target, num);
            num.pop_back();
        }
    }
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        vector<int>num;
        sort(candidates.begin(), candidates.end()); 
        permutate(candidates, 0, 0, target, num);
        return vector<vector<int>>(res.begin(), res.end());
    }
};
```

[131. Palindrome Partitioning](https://leetcode.cn/problems/palindrome-partitioning/)

题目：Given a string s, partition s such that every substring of the partition is a palindrome. Return all possible palindrome partitioning of s.

优化：将所有可能的substring是否为回文串提前算好