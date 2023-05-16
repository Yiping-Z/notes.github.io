[110. Balanced Binary Tree](https://leetcode.cn/problems/balanced-binary-tree/)
求高度（该节点到叶子节点），后序遍历
```cpp
class Solution {
public:
    int getHeight(TreeNode* root) {
        if(!root) {
            return 0;
        }
        int l = getHeight(root->left);
        if (l == -1) {
            return -1;
        }
        int r = getHeight(root->right);
        if (r == -1) {
            return -1;
        }
        return abs(l - r) <= 1 ? 1 + max(l, r) : -1;
    }
    bool isBalanced(TreeNode* root) {
        return getHeight(root) == -1 ? false : true;
    }
};
```
[257. Binary Tree Paths](https://leetcode.cn/problems/binary-tree-paths/)
void traversal(TreeNode* cur, **string path,** vector<string>& result)
每次都是复制赋值，不用使用引用 (相当于backtrack)

迭代
如果是模拟前中后序遍历就用栈，如果是适合层序遍历就用队列，当然还是其他情况，那么就是 先用队列试试行不行，不行就用栈