---
hide:
  - toc
title: Day 27 贪心
---

贪心的本质是选择每一阶段的局部最优，从而达到全局最优。<br>
验证是否可以用贪心：举反例

- 将问题分解为若干个子问题
- 找出适合的贪心策略
- 求解每一个子问题的最优解
- 将局部最优解堆叠成全局最优解

[455. Assign Cookies](https://leetcode.cn/problems/assign-cookies/)

**题目**：假设你是一位很棒的家长，想要给你的孩子们一些小饼干。但是，每个孩子最多只能给一块饼干。

对每个孩子 i，都有一个胃口值  g[i]，这是能让孩子们满足胃口的饼干的最小尺寸；并且每块饼干 j，都有一个尺寸 s[j] 。如果 s[j] >= g[i]，我们可以将这个饼干 j 分配给孩子 i ，这个孩子会得到满足。你的目标是尽可能满足越多数量的孩子，并输出这个最大数值。

**思路**：这里的局部最优就是大饼干喂给胃口大的，充分利用饼干尺寸喂饱一个，全局最优就是喂饱尽可能多的小孩。


[376. Wiggle Subsequence](https://leetcode.cn/problems/wiggle-subsequence/)

**题目**：如果连续数字之间的差严格地在正数和负数之间交替，则数字序列称为摆动序列。第一个差（如果存在的话）可能是正数或负数。少于两个元素的序列也是摆动序列。

例如， [1,7,4,9,2,5] 是一个摆动序列，因为差值 (6,-3,5,-7,3)  是正负交替出现的。相反, [1,4,7,2,5]  和  [1,7,4,5,5] 不是摆动序列，第一个序列是因为它的前两个差值都是正数，第二个序列是因为它的最后一个差值为零。

给定一个整数序列，返回作为摆动序列的最长子序列的长度。 通过从原始序列中删除一些（也可以不删除）元素来获得子序列，剩下的元素保持其原始顺序。

**思路**：

局部最优：删除单调坡度上的节点（不包括单调坡度两端的节点），那么这个坡度就可以有两个局部峰值。

整体最优：整个序列有最多的局部峰值，从而达到最长摆动序列。

![在这里插入图片描述](https://img-blog.csdnimg.cn/a2a346712a0e48dc80944b5a43fcf4f7.png)

- 情况一：上下坡中有平坡
记录峰值的条件是： (preDiff <= 0 && curDiff > 0) || (preDiff >= 0 && curDiff < 0)
- 情况二：数组首尾两端
result 初始为 1（默认最右面有一个峰值），此时 curDiff > 0 && preDiff <= 0，那么 result++（计算了左面的峰值），最后得到的 result 就是 2（峰值个数为 2 即摆动序列长度为 2）
- 情况三：单调坡中有平坡
只需要在 这个坡度 摆动变化的时候，更新 prediff 就行，这样 prediff 在 单调区间有平坡的时候 就不会发生变化，造成我们的误判

```cpp
class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
        if (nums.size() <= 1) {
            return nums.size();
        }
        // 默认序列最右边有一个峰值
        int cnt = 1;
        int currD = 0;
        int preD = 0;
        for (int i = 0; i < nums.size() - 1; i++) {
            currD = nums[i + 1] - nums[i];
            if ((preD <= 0 && currD > 0) || (preD >= 0 && currD < 0)) {
                cnt++;
                // 只在拍动变化时更新prevD
                preD = currD;
            }
        }
        return cnt;
    }
};
```
动态规划

```cpp
class Solution {
public:
    int dp[1005][2];
    int wiggleMaxLength(vector<int>& nums) {
        memset(dp, 0, sizeof dp);
        // dp[i][0]，表示考虑前 i 个数，第 i 个数作为山峰的摆动子序列的最长长度
        // dp[i][1]，表示考虑前 i 个数，第 i 个数作为山谷的摆动子序列的最长长度
        dp[0][0] = dp[0][1] = 1;
        for (int i = 1; i < nums.size(); ++i) {
            dp[i][0] = dp[i][1] = 1;
            for (int j = 0; j < i; ++j) {
            	// 表示将 nums[i]接到前面某个山峰后面，作为山谷。
                if (nums[j] > nums[i]) dp[i][1] = max(dp[i][1], dp[j][0] + 1);
            }
            for (int j = 0; j < i; ++j) {
            	// 表示将 nums[i]接到前面某个山谷后面，作为山峰。
                if (nums[j] < nums[i]) dp[i][0] = max(dp[i][0], dp[j][1] + 1);
            }
        }
        return max(dp[nums.size() - 1][0], dp[nums.size() - 1][1]);
    }
};
```
进阶

可以用两棵线段树来维护区间的最大值<br>
每次更新dp[i][0]，则在tree1的nums[i]位置值更新为dp[i][0]<br>
每次更新dp[i][1]，则在tree2的nums[i]位置值更新为dp[i][1]<br>
则 dp 转移方程中就没有必要 j 从 0 遍历到 i-1，可以直接在线段树中查询指定区间的值即可。<br>
时间复杂度：O(nlog n)<br>
空间复杂度：O(n)

