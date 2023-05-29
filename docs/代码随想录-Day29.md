---
hide:
  - toc
title: Day 29 贪心
---

[1005. K 次取反后最大化的数组和](https://leetcode.cn/problems/maximize-sum-of-array-after-k-negations/)

**题目**：给你一个整数数组 nums 和一个整数 k ，按以下方法修改该数组：

选择某个下标 i 并将 nums[i] 替换为 -nums[i] 。
重复这个过程恰好 k 次。可以多次选择同一个下标 i 。

以这种方式修改数组后，返回数组 可能的最大和 。


```cpp
class Solution {
static bool cmp(int a, int b) {
    return abs(a) > abs(b);
}
public:
    int largestSumAfterKNegations(vector<int>& A, int K) {
        sort(A.begin(), A.end(), cmp);       // 第一步 用绝对值大小排序
        for (int i = 0; i < A.size(); i++) { // 第二步
            if (A[i] < 0 && K > 0) {
                A[i] *= -1;
                K--;
            }
        }
        if (K % 2 == 1) A[A.size() - 1] *= -1; // 第三步 绝对值最小的取反
        int result = 0;
        for (int a : A) result += a;        // 第四步
        return result;
    }
};
```
[134. 加油站](https://leetcode.cn/problems/gas-station/)

**题目**：在一条环路上有 n 个加油站，其中第 i 个加油站有汽油 gas[i] 升。

你有一辆油箱容量无限的的汽车，从第 i 个加油站开往第 i+1 个加油站需要消耗汽油 cost[i] 升。你从其中的一个加油站出发，开始时油箱为空。

给定两个整数数组 gas 和 cost ，如果你可以绕环路行驶一周，则返回出发时加油站的编号，否则返回 -1 。如果存在解，则 保证 它是 唯一 的。

**思路**：局部最优：当前累加rest[i]的和curSum一旦小于0，起始位置至少要是i+1，因为从i之前开始一定不行。全局最优：找到可以跑一圈的起始位置。

```cpp
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int curr = 0;
        int total = 0;
        int idx = 0;
        for (int i = 0; i < gas.size(); i++) {
            curr += gas[i] - cost[i];
            total += gas[i] - cost[i];
            if (curr < 0) {
                idx = i + 1;
                curr = 0;
            }
        }
        if (total < 0) {
            return -1;
        }
        return idx;
    }
};
```

[135. 分发糖果](https://leetcode.cn/problems/candy/)

**题目**：n 个孩子站成一排。给你一个整数数组 ratings 表示每个孩子的评分。

你需要按照以下要求，给这些孩子分发糖果：

每个孩子至少分配到 1 个糖果。
相邻两个孩子评分更高的孩子会获得更多的糖果。
请你给每个孩子分发糖果，计算并返回需要准备的 最少糖果数目 。

**思路**：
一次是从左到右遍历，只比较右边孩子评分比左边大的情况。
一次是从右到左遍历，只比较左边孩子评分比右边大的情况。

```cpp
class Solution {
public:
    int candy(vector<int>& ratings) {
        vector<int>res(ratings.size(), 1);
        for (int i = 1; i < ratings.size(); i++) {
            if (ratings[i + 1] > ratings[i]) {
                res[i + 1] = max(res[i + 1], res[i] + 1);
            }
        }
        for (int i = ratings.size() - 2; i >= 0; i--) {
            if (ratings[i + 1] < ratings[i]) {
                res[i] = max(res[i], res[i + 1] + 1);
            }
        }
        return accumulate(res.begin(), res.end(), 0);
    }
};
```
