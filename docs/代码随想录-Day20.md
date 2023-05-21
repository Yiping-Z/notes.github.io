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

[108. Convert Sorted Array to Binary Search Tree](https://leetcode.cn/problems/convert-sorted-array-to-binary-search-tree/)

思路：题目要求是balanced BST, 如果不是BST, 就会变成只有左子树/右子树的链表。
```cpp
class Solution {
public:
    TreeNode* traverse(vector<int>&nums, int l, int r) {
        if (l > r) {
            return NULL;
        }
        // 如果数组长度为偶数，中间位置有两个元素，取靠左边的
        int m = l + (r - l) / 2;
        TreeNode* root = new TreeNode(nums[m]);
        root->left = traverse(nums, l, m - 1);
        root->right = traverse(nums, m + 1, r);
        return root;
    }
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        return traverse(nums, 0, nums.size() - 1);
    }
};
```

总结：

涉及到二叉树的构造，无论普通二叉树还是二叉搜索树一定前序，都是先构造中节点。

求普通二叉树的属性，一般是后序，一般要通过递归函数的返回值做计算。(前序求深度，方便让父节点指向子节点)

求二叉搜索树的属性，一定是中序了，要不白瞎了有序性了。