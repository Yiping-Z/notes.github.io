---
hide:
  - toc
title: Day 25 回溯
---
[491. Non-decreasing Subsequences](https://leetcode.cn/problems/non-decreasing-subsequences/)

题目：Given an integer array nums, return all the different possible non-decreasing subsequences of the given array with at least two elements. You may return the answer in any order.

```cpp
class Solution {
public:
    vector<vector<int>>res;
    void permutate(int idx, vector<int>curr, vector<int>nums) {
        if (curr.size() >= 2) {
                res.push_back(curr);
        }
        if (idx >= nums.size()) {
            return;
        }
        // 用数组优化
        int used[201] = {0};
        for (int i = idx; i < nums.size(); i++) {
        	// 同一父节点下的同层上使用过的元素不能再使用
            if ((!curr.empty() && nums[i] < curr.back()) || used[nums[i] + 100]) {
                continue;
            }
            curr.push_back(nums[i]);
            used[nums[i] + 100] = 1;
            permutate(i + 1, curr, nums);
            curr.pop_back();
        }
    }
    vector<vector<int>> findSubsequences(vector<int>& nums) {
        vector<int>curr;
        permutate(0, curr, nums);
        return res;
    }
};
```

[47. Permutations II](https://leetcode.cn/problems/permutations-ii/)

题目：Given a collection of numbers, nums, that might contain duplicates, return all possible unique permutations in any order.

```cpp
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking (vector<int>& nums, vector<bool>& used) {
        // 此时说明找到了一组
        if (path.size() == nums.size()) {
            result.push_back(path);
            return;
        }
        for (int i = 0; i < nums.size(); i++) {
            // used[i - 1] == true，说明同一树枝nums[i - 1]使用过
            // used[i - 1] == false，说明同一树层nums[i - 1]使用过
            // 如果同一树层nums[i - 1]使用过则直接跳过
            if (i > 0 && nums[i] == nums[i - 1] && used[i - 1] == false) {
                continue;
            }
            if (used[i] == false) {
                used[i] = true;
                path.push_back(nums[i]);
                backtracking(nums, used);
                path.pop_back();
                used[i] = false;
            }
        }
    }
public:
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        result.clear();
        path.clear();
        sort(nums.begin(), nums.end()); // 排序
        vector<bool> used(nums.size(), false);
        backtracking(nums, used);
        return result;
    }
};
```