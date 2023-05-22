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


[17. Letter Combinations of a Phone Number](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/)

Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent. Return the answer in any order.

A mapping of digits to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.
![在这里插入图片描述](https://img-blog.csdnimg.cn/e3c08be9ac404db691ce80402e85dced.png)


```cpp
class Solution {
public:
    vector<string>mp;
    vector<string>res;
    void permutate(string digits, string str, int idx) {
        if (str.length() == digits.length()) {
            res.push_back(str);
            return;
        }
        for (auto x: mp[digits[idx] - '0']) {
            str += x;
            permutate(digits, str, idx + 1);
            str.pop_back();
        }
    }
    vector<string> letterCombinations(string digits) {
        if (digits.length() == 0) {
            return {};
        }
        mp = vector<string>(10);
        mp[2] = "abc";
        mp[3] = "def";
        mp[4] = "ghi";
        mp[5] = "jkl";
        mp[6] = "mno";
        mp[7] = "pqrs";
        mp[8] = "tuv";
        mp[9] = "wxyz";
        string str = "";
        permutate(digits, str, 0);
        return res;
    }
};
```
