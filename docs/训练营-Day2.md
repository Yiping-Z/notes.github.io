---
hide:
  - toc
title: Day 2
---
[977.有序数组的平方](https://leetcode.cn/problems/squares-of-a-sorted-array/)

题目： 给你一个递增数组nums（包含负数），返回一个数组，且数组要求里面的元素是给定数组nums里面元素的平方，还要求递增

```cpp
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        vector<int>res(nums.size());
        int l = 0;
        int r = nums.size() - 1;
        int idx = r;
        while (l <= r) {
            // 每次比较两端的值，因为最大值只会在两端出现
            if (abs(nums[l]) > abs(nums[r])) {
                // 放进结果数组的末尾
                res[idx] = (nums[l] * nums[l]);
                l++;
            } else {
                res[idx] = (nums[r] * nums[r]);
                r--;
            }
            idx--;
        }
        return res;
    }
};
```

[209.长度最小的子数组](https://leetcode.cn/problems/minimum-size-subarray-sum/submissions/)

题目：给你一个数组和target，求出一个连续子数组，并且这个子数组相加要大于target，返回子数组最短的长度

思路：滑动窗口

```cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int res = INT_MAX;
        int l = 0;
        int r = 0;
        int sum = 0;
        while (r < nums.size()) {
            sum += nums[r];
            r++;
            while (sum - nums[l] >= target) {
                sum -= nums[l];
                l++;
            }
            if (sum >= target) {
                res = min(res, r - l);
            }
        }
        return res == INT_MAX ? 0 : res;
    }
};
```

[59. 螺旋矩阵II](https://leetcode.cn/problems/spiral-matrix-ii/)

题目： 给你一个n，生成NxN的矩阵，并且要求你顺时针填入1，2，3，4，5...

```cpp
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>>dir = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
        vector<vector<int>>res(n, vector<int>(n));
        vector<vector<bool>>visited(n, vector<bool>(n, false));
        int num = 1;
        int x = 0;
        int y = 0;
        int idx = 0;
        while (num <= n * n) {
            res[x][y] = num;
            visited[x][y] = true;
            int xi = x + dir[idx][0];
            int yi = y + dir[idx][1];
            if (!(xi >= 0 && xi < n && yi >= 0 && yi < n) || visited[xi][yi]) {
                idx =  (idx + 1) % 4;
                xi = x + dir[idx][0];
                yi = y + dir[idx][1];
            }
            x = xi;
            y = yi;
            num++;
        }
        return res;
    }
};
```
