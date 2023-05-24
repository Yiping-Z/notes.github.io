---
hide:
  - toc
title: Day 24 回溯
---
[93. Restore IP Addresses](https://leetcode.cn/problems/restore-ip-addresses/)

题目：Given a string s containing only digits, return all possible valid IP addresses that can be formed by inserting dots into s. You are not allowed to reorder or remove any digits in s. You may return the valid IP addresses in any order.

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
