---
hide:
  - toc
title: Day 20 二叉搜索树
---
[669. Trim a Binary Search Tree](https://leetcode.cn/problems/trim-a-binary-search-tree/)
```cpp
class Solution {
public:
    TreeNode* trimBST(TreeNode* root, int low, int high) {
        if (!root) {
            return NULL;
        }
        if (root->val > high) {
            return trimBST(root->left, low, high);
        } 
        if (root->val < low) {
            return trimBST(root->right, low, high);
        }
        root->left = trimBST(root->left, low, high);
        root->right = trimBST(root->right, low, high);
        return root;
     }
};
```