---
hide:
  - toc
title: Day 24 回溯
---
[93. Restore IP Addresses](https://leetcode.cn/problems/restore-ip-addresses/)

题目：Given a string s containing only digits, return all possible valid IP addresses that can be formed by inserting dots into s. <br>You are not allowed to reorder or remove any digits in s. You may return the valid IP addresses in any order.

```cpp
class Solution {
public:
    vector<string>res;
    bool isValid(int l, int r, string s) {
        if (r - l > 0 && s[l] == '0') {
            return false;
        }
        int num = stoi(s.substr(l, r - l + 1));
        return num >= 0 && num <= 255;
    }
    void permutate(int idx, string curr, string s, int count) {
        if (idx == s.length()) {
            if (count == 4) {
                res.push_back(curr.substr(0, curr.length() - 1));
            }
            return;
        }
        for (int i = idx; i < idx + 3 && i < s.length(); i++) {
            if (isValid(idx, i, s) && count < 4) {
                curr += (s.substr(idx, i - idx + 1) + ".");
                permutate(i + 1, curr, s, count + 1);
                curr = curr.substr(0, curr.length() - (i - idx + 2));
            }
        }
    }
    vector<string> restoreIpAddresses(string s) {
        permutate(0, "", s, 0);
        return res;
    }
};
```

[78. Subsets](https://leetcode.cn/problems/subsets/)

题目： Given an integer array nums of unique elements, return all possible subsets (the power set).

The solution set must not contain duplicate subsets. Return the solution in any order.

思路： 合问题和分割问题都是收集树的叶子节点，而子集问题是找树的所有节点

```cpp
class Solution {
public:
    vector<vector<int>>res;
    void permutate(int idx, vector<int>&nums, vector<int>num) {
        res.push_back(num);
        if (idx == nums.size()) {
            return;
        }
        for (int i = idx; i < nums.size(); i++) {
            num.push_back(nums[i]);
            permutate(i + 1, nums, num);
            num.pop_back();
        }
    }
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<int>num;
        permutate(0, nums, num);
        return res;
    }
};
```
[90. Subsets II](https://leetcode.cn/problems/subsets-ii/)

题目：Given an integer array nums that may contain duplicates, return all possible subsets (the power set).

The solution set must not contain duplicate subsets. Return the solution in any order.

```cpp
class Solution {
public:
    vector<vector<int>>res;
    void permutate(int idx, vector<int>&nums, vector<int>num) {
        res.push_back(num);
        if (idx == nums.size()) {
            return;
        }
        for (int i = idx; i < nums.size(); i++) {
        	// 	去重
            if (i != idx && nums[i - 1] == nums[i]) {
                continue;
            }
            num.push_back(nums[i]);
            permutate(i + 1, nums, num);
            num.pop_back();
        }
    }
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        vector<int>num;
        sort(nums.begin(), nums.end());
        permutate(0, nums, num);
        return res;
    }
};
```
