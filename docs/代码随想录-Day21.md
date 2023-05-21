---
hide:
  - toc
title: Day 21 回溯
---

**回溯法**，一般可以解决如下几种问题：

- 组合问题：N个数里面按一定规则找出k个数的集合
- 切割问题：一个字符串按一定规则有几种切割方式
- 子集问题：一个N个数的集合里有多少符合条件的子集
- 排列问题：N个数按一定规则全排列，有几种排列方式
- 棋盘问题：N皇后，解数独等等

回溯法解决的问题都可以抽象为树形结构。<br>
因为回溯法解决的都是在集合中递归查找子集，集合的大小就构成了树的宽度，递归的深度，都构成的树的深度。
![在这里插入图片描述](https://img-blog.csdnimg.cn/f0d51476bddb409a8ef105575b390b96.png) <br>
for循环可以理解是横向遍历，backtracking（递归）就是纵向遍历

[77. Combinations](https://leetcode.cn/problems/combinations/)
```cpp
class Solution {
public:
    vector<vector<int>>res;
    void permutate(vector<int>&num, int curr, int end, int k) {
        if (num.size() == k) {
            res.push_back(num);
            return;
        }
        // 剪枝优化：
        // for (int i = curr; i <= n - (k - num.size()) + 1; i++)  {
        // i为本次搜索的起始位置
        for (int i = curr; i <= end; i++) {
            num.push_back(i);
            permutate(num, i + 1, end, k);
            num.pop_back();
        }
    }
    vector<vector<int>> combine(int n, int k) {
        vector<int>temp;
        permutate(temp, 1, n, k);
        return res;
    }
};
```
模版：

```cpp
void backtracking(参数) {
    if (终止条件) {
        存放结果;
        return;
    }

    for (选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) {
        处理节点;
        backtracking(路径，选择列表); // 递归
        回溯，撤销处理结果
    }
}
```
