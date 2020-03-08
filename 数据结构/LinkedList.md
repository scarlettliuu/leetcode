<!-- GFM-TOC -->
* [1. 找出两个链表的交点](#1-找出两个链表的交点)
<!-- GFM-TOC -->

#  1. 找出两个链表的交点

160\. Intersection of Two Linked Lists (Easy)
[Leetcode](https://leetcode.com/problems/intersection-of-two-linked-lists/description/)

**题目**  
编写一个程序，找到两个单链表相交的起始节点。  
如下面的两个链表：  
```html
A:          a1 → a2
                    ↘
                      c1 → c2 → c3
                    ↗
B:    b1 → b2 → b3
```  
在节点 c1 开始相交。  
**分析**  
把两个链表相同的部分记为c，相交节点记为*, 则第一个链表记为 a * c, 第二个链表记为b * c  
两个虚拟链表AB, BA可以记为：  
a * c b * c  
b * c a * c  
a, b长度不一定同，但是第二个 * 之前两个链表的长度相同，  
由此两个虚拟链表走相同步数可以到第二个 * 处  

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode *l1 = headA, *l2 = headB;
        while(l1!=l2){
            l1=(l1==NULL)?headB:l1->next;
            l2=(l2==NULL)?headA:l2->next;
        }
        return l1;
    }
};
```
