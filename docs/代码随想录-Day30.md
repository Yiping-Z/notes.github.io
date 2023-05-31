---
hide:
  - toc
title: Day 30 贪心
---
### [860. 柠檬水找零](https://leetcode.cn/problems/lemonade-change/)

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

### [406. 根据身高重建队列](https://leetcode.cn/problems/queue-reconstruction-by-height/)

**题目**：假设有打乱顺序的一群人站成一个队列，数组 people 表示队列中一些人的属性（不一定按顺序）。每个 people[i] = [hi, ki] 表示第 i 个人的身高为 hi ，前面 正好 有 ki 个身高大于或等于 hi 的人。

请你重新构造并返回输入数组 people 所表示的队列。返回的队列应该格式化为数组 queue ，其中 queue[j] = [hj, kj] 是队列中第 j 个人的属性（queue[0] 是排在队列前面的人）。

**思路**：从高到低的顺序排，插入ki位，因为后插入的数小，不会影响已经插入的数。List底层实现是Linked List, 插入耗时比vector短。

局部最优：优先按身高高的people的k来插入。插入操作过后的people满足队列属性

全局最优：最后都做完插入操作，整个队列满足题目队列属性

```cpp
class Solution {
public:
    static bool cmp(vector<int>&a, vector<int>&b) {
        if (a[0] == b[0]) {
            return a[1] < b[1];
        }
        return a[0] > b[0];
    }
    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        sort(people.begin(), people.end(), cmp);
        list<vector<int>>qu;
        for (int i = 0; i < people.size(); i++) {
            int pos = people[i][1];
            list<vector<int>>::iterator it = qu.begin();
            while (pos--) {
                it++;
            }
            qu.insert(it, people[i]);
        }
        return vector<vector<int>>(qu.begin(), qu.end());
    }
};
```

### [452. 用最少数量的箭引爆气球](https://leetcode.cn/problems/minimum-number-of-arrows-to-burst-balloons/)

**题目**：有一些球形气球贴在一堵用 XY 平面表示的墙面上。墙面上的气球记录在整数数组 points ，其中points[i] = [xstart, xend] 表示水平直径在 xstart 和 xend之间的气球。你不知道气球的确切 y 坐标。

一支弓箭可以沿着 x 轴从不同点 完全垂直 地射出。在坐标 x 处射出一支箭，若有一个气球的直径的开始和结束坐标为 xstart，xend， 且满足  xstart ≤ x ≤ xend，则该气球会被 引爆 。可以射出的弓箭的数量 没有限制 。 弓箭一旦被射出之后，可以无限地前进。

给你一个数组 points ，返回引爆所有气球所必须射出的 最小 弓箭数 。

**思路**：排序，让气球尽可能的重叠，更新右边边界的最小值。

```cpp
class Solution {
public:
    int findMinArrowShots(vector<vector<int>>& points) {
        sort(points.begin(), points.end());
        int cnt = 1;
        for (int i = 1; i < points.size(); i++) {
            if (points[i][0] > points[i - 1][1]) {
                cnt++;
            } else {
                points[i][1] = min(points[i][1], points[i - 1][1]);
            }
        }
        return cnt;
    }
};
```