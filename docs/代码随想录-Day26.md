---
hide:
  - toc
title: Day 26 回溯
---

[332. Reconstruct Itinerary](https://leetcode.cn/problems/reconstruct-itinerary/)

题目：You are given a list of airline tickets where tickets[i] = [fromi, toi] represent the departure and the arrival airports of one flight. Reconstruct the itinerary in order and return it.

All of the tickets belong to a man who departs from "JFK", thus, the itinerary must begin with "JFK". If there are multiple valid itineraries, you should return the itinerary that has the smallest lexical order when read as a single string.

```cpp
class Solution {
public:
    unordered_map<string, multiset<string>>adj;
    vector<string>res;
    int n = 0;
    void dfs(string src) {
        if (res.size() == n) {
            return;
        }
        while (adj[src].size() != 0) {
            string neighbor = *adj[src].begin();
            adj[src].erase(adj[src].begin());
            dfs(neighbor);
        }
        res.push_back(src);
    }
    vector<string> findItinerary(vector<vector<string>>& tickets) {
        n = tickets.size() + 1;
        for (auto x: tickets) {
            adj[x[0]].insert(x[1]);
        }
        dfs("JFK");
        return vector<string>(res.rbegin(), res.rend());
    }
};
```