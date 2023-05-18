---
hide:
  - toc
title: Day 3
---
[移除链表元素](https://leetcode.cn/problems/remove-linked-list-elements/submissions/)

题目： 给你一个链表和一个val，删除所有节点值等于val的节点，返回链表

思路：dummy处理头节点，access next前记得先判定next不为空

```cpp
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        if (!head) {
            return NULL;
        }
        ListNode* dummy = new ListNode(0, head);
        ListNode* copy = dummy;
        while (dummy) {
            if (dummy->next && dummy->next->val == val) {
                dummy->next = dummy->next->next;
            } else {
                dummy = dummy->next;
            }
        }
        return copy->next;
    }
};
```
题目：[设计链表](https://leetcode.cn/problems/design-linked-list/)
```cpp
class MyLinkedList {
public:
    struct Node {
        int val;
        Node* next;
        Node(int v): val(v), next(NULL) {}
    };
    Node* dummy = new Node(0);
    int size = 0;
    MyLinkedList() {

    }
    
    int get(int index) {
        if (index >= size) {
            return -1;
        }
        Node* curr = dummy->next;
        while (index && curr) {
            curr = curr->next;
            index--;
        }
        return curr->val;
    }
    
    void addAtHead(int val) {
        Node* node = new Node(val);
        node->next = dummy->next;
        dummy->next = node;
        size++;
    }
    
    void addAtTail(int val) {
        Node* curr = dummy;
        while (curr && curr->next) {
            curr = curr->next;
        }
        curr->next = new Node(val);
        size++;
    }
    
    void addAtIndex(int index, int val) {
        if (index > size) {
            return;
        } else if (index == size) {
            addAtTail(val);
            return;
        }
        Node* curr = dummy;
        while (index && curr) {
            curr = curr->next;
            index--;
        }
        Node* node = new Node(val);
        node->next = curr->next;
        curr->next = node;
        size++;
    }
    
    void deleteAtIndex(int index) {
        if (index >= size) {
            return;
        } 
        Node* curr = dummy;
        while (index && curr) {
            curr = curr->next;
            index--;
        }
        curr->next = curr->next->next;
        size--;
    }
};
```

[反转链表](https://leetcode.cn/problems/reverse-linked-list/)

题目： 给你一个链表，翻转一下，返回头节点
```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* prev = NULL;
        while (head) {
            ListNode* next = head->next;
            head->next = prev;
            prev = head;
            head = next;
        }
        return prev;
    }
};
```