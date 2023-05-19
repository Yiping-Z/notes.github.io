---
hide:
  - toc
title: Day 18 二叉树, 二叉搜索树
---
[530. Minimum Absolute Difference in BST](https://leetcode.cn/problems/minimum-absolute-difference-in-bst/)

思路：中序遍历，按顺序比较节点大小。用prev和curr比较求最小差值
```cpp
class Solution {
public:
    int getMinimumDifference(TreeNode* root) {
        stack<TreeNode*> st;
        TreeNode* cur = root;
        TreeNode* pre = NULL;
        int result = INT_MAX;
        while (cur != NULL || !st.empty()) {
            if (cur != NULL) { // 指针来访问节点，访问到最底层
                st.push(cur); // 将访问的节点放进栈
                cur = cur->left;                // 左
            } else {
                cur = st.top();
                st.pop();
                if (pre != NULL) {              // 中
                    result = min(result, cur->val - pre->val);
                }
                pre = cur;
                cur = cur->right;               // 右
            }
        }
        return result;
    }
};
```
[501. Find Mode in Binary Search Tree](https://leetcode.cn/problems/find-mode-in-binary-search-tree/)

```cpp
class Solution {
public:
    int freq = 0;
    int prev = INT_MIN;
    int maxF = INT_MIN;
    vector<int>res;
    void traverse(TreeNode* root) {
        if (!root) {
            return;
        }
        traverse(root->left);
        if (prev == INT_MIN) {
            freq = 1;
        } else if (prev == root->val) {
            freq++;
        } else {
            freq = 1;
        }
        prev = root->val;
        if (freq > maxF) {
            maxF = freq;
            res.clear();
            res.push_back(root->val);
        } else if (freq == maxF) {
            res.push_back(root->val);
        }
        traverse(root->right);
    }
    vector<int> findMode(TreeNode* root) {
        traverse(root);
        return res;
    }
};
```

[236. Lowest Common Ancestor of a Binary Tree](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/)

思路：从底向上遍历，后序遍历
```cpp
\\ 搜索边
if (递归函数(root->left)) return ;
if (递归函数(root->right)) return ;
\\ 搜索整个树
left = 递归函数(root->left);  // 左
right = 递归函数(root->right); // 右
left与right的逻辑处理;         // 中 
```

```cpp
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (!root || root->val == p->val || root->val ==  q->val) {
            return root;
        }
        TreeNode* l = lowestCommonAncestor(root->left, p, q);
        TreeNode* r = lowestCommonAncestor(root->right, p, q);
        if (l && r) {
            return root;
        } else if (l) {
            return l;
        }
        return r;
    }   
};
```
