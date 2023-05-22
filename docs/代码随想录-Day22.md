---
hide:
  - toc
title: Day 22 回溯
---

[216. Combination Sum III](https://leetcode.cn/problems/combination-sum-iii/)

题目：
Find all valid combinations of k numbers that sum up to n such that the following conditions are true:

- Only numbers 1 through 9 are used.
- Each number is used at most once.

Return a list of all possible valid combinations. The list must not contain the same combination twice, and the combinations may be returned in any order.

```cpp
class Solution {
public:
    vector<vector<int>>res;
    void permutate(int idx, int curr, int size, int sum, vector<int>num) {
        if (curr >= sum || num.size() == size) {
            if (curr == sum && num.size() == size) {
                res.push_back(num);
            }
            return;
        }
        for (int i = idx; i <= 9 - (size - num.size()) + 1; i++) {
            num.push_back(i);
            permutate(i + 1, curr + i, size, sum, num);
            num.pop_back();
        }
    }
    vector<vector<int>> combinationSum3(int k, int n) {
        vector<int>num;
        permutate(1, 0, k, n, num);
        return res;
    }
};
```
