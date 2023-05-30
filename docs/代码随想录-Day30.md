---
hide:
  - toc
title: Day 30 贪心
---
[860. 柠檬水找零](https://leetcode.cn/problems/lemonade-change/)

**题目**：在柠檬水摊上，每一杯柠檬水的售价为 5 美元。

顾客排队购买你的产品，（按账单 bills 支付的顺序）一次购买一杯。

每位顾客只买一杯柠檬水，然后向你付 5 美元、10 美元或 20 美元。你必须给每个顾客正确找零，也就是说净交易是每位顾客向你支付 5 美元。

注意，一开始你手头没有任何零钱。

如果你能给每位顾客正确找零，返回 true ，否则返回 false 。

**思路**：只需要维护三种金额的数量，5，10和20。5比10更有用，优先用10。
```cpp
class Solution {
public:
    bool lemonadeChange(vector<int>& bills) {
        int fi = 0;
        int te = 0;
        int tw = 0;
        for (auto x: bills) {
            if (x == 5) {
                fi++;
            } else if (x == 10) {
                if (fi <= 0) {
                    return false;
                }
                fi--;
                te++;
            } else {
                if (fi > 0 && te > 0) {
                    fi--;
                    te--;
                    tw++;
                } else if (fi >= 3) {
                    fi -= 3;
                    tw++;
                } else {
                    return false;
                }
            }
        }
        return true;
    }
};
```
