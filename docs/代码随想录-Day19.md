---
hide:
  - toc
title: Day 19 二叉搜索树
---
完全二叉树一定是平衡二叉树，堆的排序是父节点大于子节点，<br>
而搜索树是父节点大于左孩子，小于右孩子，所以堆不是平衡二叉搜索树

[235. Lowest Common Ancestor of a Binary Search Tree](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-search-tree/)

思路：<br>
二叉树：如果node本身是自己的ancestor，那么只有left/right一边有值，另一边为空值，必须搜索整个二叉树<br>
二叉搜索树：从上向下去递归遍历，第一次遇到 cur节点是数值在[p, q]区间中，那么cur就是p和q的最近公共祖先。不需要遍历整个树。
```cpp
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (root->val > p->val && root->val > q->val) {
            return lowestCommonAncestor(root->left, p, q);
        } else if (root->val < p->val && root->val < q->val) {
            return lowestCommonAncestor(root->right, p, q);
        }
        return root;
    }
};

// 迭代
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        while(root) {
            if (root->val > p->val && root->val > q->val) {
                root = root->left;
            } else if (root->val < p->val && root->val < q->val) {
                root = root->right;
            } else return root;
        }
        return NULL;
    }
};
```

[701. Insert into a Binary Search Tree](https://leetcode.cn/problems/insert-into-a-binary-search-tree/)

```cpp
class Solution {
public:
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        if (!root) {
            // 下一层将加入节点返回，本层用root->left或者root->right将其接住
            return new TreeNode(val);
        } else if (root->val < val) {
            root->right = insertIntoBST(root->right, val);
        } else if (root->val > val) {
            root->left = insertIntoBST(root->left, val);
        }
        return root;
    }
};
```

[450. Delete Node in a BST](https://leetcode.cn/problems/delete-node-in-a-bst/)

- 没找到删除的节点，遍历到空节点直接返回了
- 找到删除的节点
	- 左右孩子都为空（叶子节点），直接删除节点， 返回NULL为根节点
	- 删除节点的左孩子为空，右孩子不为空，删除节点，右孩子补位，返回右孩子为根节点
   - 删除节点的右孩子为空，左孩子不为空，删除节点，左孩子补位，返回左孩子为根节点
  - 左右孩子节点都不为空，则将删除节点的左子树头结点（左孩子）放到删除节点的右子树的最左面节点的左孩子上，返回删除节点右孩子为新的根节点

```cpp
class Solution {
public:
    TreeNode* deleteNode(TreeNode* root, int key) {
        if (!root) {
            return root;
        } 
        if (root->val == key) {
            if (!root->left && !root->right) {
                delete root;
                return NULL;
            } else if (!root->left) {
                TreeNode* node = root->right;
                delete root;
                return node;
            } else if (!root->right) {
                TreeNode* node = root->left;
                delete root;
                return node;
            } else {
                TreeNode* cur = root->right;
                while (cur->left) {
                    cur = cur->left;
                }
                cur->left = root->left;
                TreeNode* tmp = root;
                root = root->right;
                delete tmp;
                return root;
            }
        }
        if (root->val > key) {
            root->left = deleteNode(root->left, key);
        }
        if (root->val < key) {
            root->right = deleteNode(root->right, key);
        }
        return root;
    }
};

// 普通二叉树删除节点
class Solution {
public:
    TreeNode* deleteNode(TreeNode* root, int key) {
        if (root == nullptr) return root;
        if (root->val == key) {
            if (root->right == nullptr) { // 这里第二次操作目标值：最终删除的作用
                return root->left;
            }
            TreeNode *cur = root->right;
            while (cur->left) {
                cur = cur->left;
            }
            swap(root->val, cur->val); // 这里第一次操作目标值：交换目标值其右子树最左面节点。
        }
        root->left = deleteNode(root->left, key);
        root->right = deleteNode(root->right, key);
        return root;
    }
};
```
