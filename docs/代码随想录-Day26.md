---
hide:
  - toc
title: Day 26 回溯
---

[332. Reconstruct Itinerary](https://leetcode.cn/problems/reconstruct-itinerary/)

题目：You are given a list of airline tickets where tickets[i] = [fromi, toi] represent the departure and the arrival airports of one flight. Reconstruct the itinerary in order and return it.

All of the tickets belong to a man who departs from "JFK", thus, the itinerary must begin with "JFK". If there are multiple valid itineraries, you should return the itinerary that has the smallest lexical order when read as a single string.

思路：
出发机场和到达机场会重复，需要增删元素。<br>
```unordered_map<string, multiset<string>> targets``` 遍历multiset的时候，不能删除元素，一旦删除元素，迭代器就失效了。<br>
用```unordered_map<string, map<string, int>> targets```：```unordered_map<出发机场, map<到达机场, 航班次数>> targets```

```cpp
class Solution {
public:
    unordered_map<string, map<string, int>>adj;
    vector<string>res;
    int n = 0;
    bool dfs() {
        if (res.size() == n) {
            return true;
        }
        // &: 修改map里的原值，key是constant，不可修改
        for (pair<const string, int>& x: adj[res[res.size() - 1]]) {
            if (x.second > 0) {
                res.push_back(x.first);
                x.second--;
                if (dfs()) {
                    return true;
                }
                res.pop_back();
                x.second++;
            }
        }
        return false;
    }
    vector<string> findItinerary(vector<vector<string>>& tickets) {
        n = tickets.size() + 1;
        for (auto x: tickets) {
            adj[x[0]][x[1]]++;
        }
        res.push_back("JFK");
        dfs();
        return res;
    }
};
```


[51. N-Queens](https://leetcode.cn/problems/n-queens/)

**题目**：n 皇后问题 研究的是如何将 n 个皇后放置在 n×n 的棋盘上，并且使皇后彼此之间不能相互攻击。

给你一个整数 n ，返回所有不同的 n 皇后问题 的解决方案。

每一种解法包含一个不同的 n 皇后问题 的棋子放置方案，该方案中 'Q' 和 '.' 分别代表了皇后和空位。

**思路**： 棋盘的宽度就是for循环的长度，递归的深度就是棋盘的高度
```cpp
class Solution {
public:
    vector<vector<string>>res;
    bool isValid(int r, int c, vector<string>&board, int n) {
        for (int i = 0; i < r; i++) {
            if (board[i][c] == 'Q') {
                return false;
            }
        }
        for (int i = r - 1, j = c - 1; i >= 0 && j >= 0; i--, j--) {
            if (board[i][j] == 'Q') {
                return false;
            }
        }
        for (int i = r - 1, j = c + 1; i >= 0 && j < n; i--, j++) {
            if (board[i][j] == 'Q') {
                return false;
            }
        }
        return true;
    }
    void backtrack(int n, int r, vector<string>&board) {
        if (r == n) {
            res.push_back(board);
            return;
        }
        for (int c = 0; c < n; c++) {
            if (isValid(r, c, board, n)) {
                board[r][c] = 'Q';
                backtrack(n, r + 1, board);
                board[r][c] = '.';
            }
        }
    }
    vector<vector<string>> solveNQueens(int n) {
        vector<string>board(n, string(n, '.'));
        backtrack(n, 0, board);
        return res;
    }
};
```
[37. Sudoku Solver](https://leetcode.cn/problems/sudoku-solver/)

**题目**： 编写一个程序，通过填充空格来解决数独问题。

一个数独的解法需遵循如下规则： 数字 1-9 在每一行只能出现一次。 数字 1-9 在每一列只能出现一次。 数字 1-9 在每一个以粗实线分隔的 3x3 宫内只能出现一次。 空白格用 '.' 表示。

```cpp
class Solution {
private:
bool backtracking(vector<vector<char>>& board) {
    for (int i = 0; i < board.size(); i++) {        // 遍历行
        for (int j = 0; j < board[0].size(); j++) { // 遍历列
            if (board[i][j] == '.') {
                for (char k = '1'; k <= '9'; k++) {     // (i, j) 这个位置放k是否合适
                    if (isValid(i, j, k, board)) {
                        board[i][j] = k;                // 放置k
                        if (backtracking(board)) return true; // 如果找到合适一组立刻返回
                        board[i][j] = '.';              // 回溯，撤销k
                    }
                }
                return false;  // 9个数都试完了，都不行，那么就返回false
            }
        }
    }
    return true; // 遍历完没有返回false，说明找到了合适棋盘位置了
}
bool isValid(int row, int col, char val, vector<vector<char>>& board) {
    for (int i = 0; i < 9; i++) { // 判断行里是否重复
        if (board[row][i] == val) {
            return false;
        }
    }
    for (int j = 0; j < 9; j++) { // 判断列里是否重复
        if (board[j][col] == val) {
            return false;
        }
    }
    int startRow = (row / 3) * 3;
    int startCol = (col / 3) * 3;
    for (int i = startRow; i < startRow + 3; i++) { // 判断9方格里是否重复
        for (int j = startCol; j < startCol + 3; j++) {
            if (board[i][j] == val ) {
                return false;
            }
        }
    }
    return true;
}
public:
    void solveSudoku(vector<vector<char>>& board) {
        backtracking(board);
    }
};
```

**子集问题分析**：

时间复杂度：O(2 ^ n)，因为每一个元素的状态无外乎取与不取，所以时间复杂度为O(2^n)<br>
空间复杂度：O(n)，递归深度为n，所以系统栈所用空间为O(n)，每一层递归所用的空间都是常数级别，注意代码里的result和path都是全局变量，就算是放在参数里，传的也是引用，并不会新申请内存空间，最终空间复杂度为O(n)

**排列问题分析**：

时间复杂度：O(n!)，这个可以从排列的树形图中很明显发现，每一层节点为n，第二层每一个分支都延伸了n-1个分支，再往下又是n-2个分支，所以一直到叶子节点一共就是 n * n-1 * n-2 * ..... 1 = n!。<br>
空间复杂度：O(n)，和子集问题同理。

**组合问题分析**：

时间复杂度：O(2^n)，组合问题其实就是一种子集的问题，所以组合问题最坏的情况，也不会超过子集问题的时间复杂度。<br>
空间复杂度：O(n)，和子集问题同理。

**N皇后问题分析**：

时间复杂度：O(n!) ，其实如果看树形图的话，直觉上是O(n^n)，但皇后之间不能见面所以在搜索的过程中是有剪枝的，最差也就是O（n!），n!表示n * (n-1) * .... * 1。<br>
空间复杂度：O(n)，和子集问题同理。

**解数独问题分析**：

时间复杂度：O(9^m) , m是'.'的数目。
空间复杂度：O(n ^ 2)，递归的深度是n^2

![在这里插入图片描述](https://img-blog.csdnimg.cn/4f4d79ae8b404c87b6b2700d69617dc6.png)
