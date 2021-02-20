# 0002M Add Two Numbers

You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order** and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example:**

```
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output:  465 = 807.
```

## Linked Lists 

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        carry = 0
        curr = dummyHead = ListNode(0)
        while l1 or l2 or carry:
            val1 = l1.val if l1 else 0
            val2 = l2.val if l2 else 0
            sumv = val1 + val2 + carry
            carry = sumv // 10
            curr.next = ListNode(sumv % 10)
            curr = curr.next
            l1 = l1.next if l1 else l1
            l2 = l2.next if l2 else l2
        return dummyHead.next
```

**Result:** runtime 64ms (97%), memory 14MB (98%)

Time $O(\max(m,n))$: where $m,n$ are the lengths of $l1,l2$, respectively, and the algorithm interates at most $\max(m,n)$ times.

 Space $O(\max(m,n))$: the longest possible length of new lined list is $\max(m,n)+1$. 

## Ref

* [python3 addTwoNumbers - 两数相加 - 力扣（LeetCode）](https://leetcode-cn.com/problems/add-two-numbers/solution/python3-addtwonumbers-by-tianwj/) 
* [两数相加 - 官方题解](https://leetcode-cn.com/problems/add-two-numbers/solution/liang-shu-xiang-jia-by-leetcode/#%E6%96%B9%E6%B3%95%EF%BC%9A%E5%88%9D%E7%AD%89%E6%95%B0%E5%AD%A6)

