---
hide:
  - toc
title: Day 17 二叉树, 二叉搜索树
---
[654. Maximum Binary Tree](https://leetcode.cn/problems/maximum-binary-tree/)

空指针进入递归，终止条件为空指针，空指针不进入递归，终止条件为叶子结点。<br>
终止条件：一般情况来说：如果让空节点（空指针）进入递归，就不加if，如果不让空节点进入递归，就加if限制一下， 终止条件也会相应的调整。

[617. Merge Two Binary Trees](https://leetcode.cn/problems/merge-two-binary-trees/submissions/)
```cpp
// 递归
class Solution {
public:
    TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2) {
        TreeNode* root;
        if (!root1 && !root2) {
            return NULL;
        }
        if (!root1 || !root2) {
            if (!root1) {
                return root2;
            }
            return root1;
        } 
        root1->val += root2->val;
        root1->left = mergeTrees(root1->left, root2->left);
        root1->right = mergeTrees(root1->right, root2->right);
        return root1;
    }
};
```

```cpp
// 迭代：不需要backtrack，方向已经确定好了
class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int val) {
        while (root != NULL) {
            if (root->val > val) root = root->left;
            else if (root->val < val) root = root->right;
            else return root;
        }
        return NULL;
    }
};
```
[98. Validate Binary Search Tree](https://leetcode.cn/problems/validate-binary-search-tree/)

中序遍历：把二叉树转为有序数组
不能单纯的比较左节点小于中间节点，右节点大于中间节点就完事了。<br>
要比较的是 左子树所有节点小于中间节点，右子树所有节点大于中间节点。

```cpp
// 避免设最小值
class Solution {
public:
    TreeNode* pre = NULL; // 用来记录前一个节点
    bool isValidBST(TreeNode* root) {
        if (root == NULL) return true;
        bool left = isValidBST(root->left);

        if (pre != NULL && pre->val >= root->val) return false;
        pre = root; // 记录前一个节点

        bool right = isValidBST(root->right);
        return left && right;
    }
};
```

```cpp
// 迭代
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        stack<TreeNode*> st;
        TreeNode* cur = root;
        TreeNode* pre = NULL; // 记录前一个节点
        while (cur != NULL || !st.empty()) {
            if (cur != NULL) {
                st.push(cur);
                cur = cur->left;                // 左
            } else {
                cur = st.top();                 // 中
                st.pop();
                if (pre != NULL && cur->val <= pre->val)
                return false;
                pre = cur; //保存前一个访问的结点

                cur = cur->right;               // 右
            }
        }
        return true;
    }
};
```