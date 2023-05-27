---
hide:
  - toc
title: Day 26 回溯
---

[332. Reconstruct Itinerary](https://leetcode.cn/problems/reconstruct-itinerary/)

题目：You are given a list of airline tickets where tickets[i] = [fromi, toi] represent the departure and the arrival airports of one flight. Reconstruct the itinerary in order and return it.

All of the tickets belong to a man who departs from "JFK", thus, the itinerary must begin with "JFK". If there are multiple valid itineraries, you should return the itinerary that has the smallest lexical order when read as a single string.

思路：
出发机场和到达机场会重复，需要增删元素。<br>
```unordered_map<string, multiset<string>> targets``` 遍历multiset的时候，不能删除元素，一旦删除元素，迭代器就失效了。<br>
用```unordered_map<string, map<string, int>> targets```：```unordered_map<出发机场, map<到达机场, 航班次数>> targets```

```cpp
class Solution {
public:
    unordered_map<string, map<string, int>>adj;
    vector<string>res;
    int n = 0;
    bool dfs() {
        if (res.size() == n) {
            return true;
        }
        // &: 修改map里的原值，key是constant，不可修改
        for (pair<const string, int>& x: adj[res[res.size() - 1]]) {
            if (x.second > 0) {
                res.push_back(x.first);
                x.second--;
                if (dfs()) {
                    return true;
                }
                res.pop_back();
                x.second++;
            }
        }
        return false;
    }
    vector<string> findItinerary(vector<vector<string>>& tickets) {
        n = tickets.size() + 1;
        for (auto x: tickets) {
            adj[x[0]][x[1]]++;
        }
        res.push_back("JFK");
        dfs();
        return res;
    }
};
```
