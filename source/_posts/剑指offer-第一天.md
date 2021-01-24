title: 剑指offer 第一天
tags: 剑指offer
category: 刷题
date: 2020-07-06 22:10:47
---
### JZ16 合并两个排序的链表
**题目描述：**
> 输入两个单调递增的链表，输出两个链表合成后的链表，当然我们需要合成后的链表满足单调不减规则。

**解法一思路：**
使用迭代的方法。先判断临界情况，依次判断输入的两个链表是否为空指针，如果有其中一个为空指针链表，则返回另一个有序的链表。

然后初始化一个新链表，依次比较两个有序链表的值，哪个链表的值较小，则新链表的指针指向该值。继续循环，依次比较，最终返回单调递增的新链表。
```
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None
        
        
class Solution2:
    # 返回合并后列表
    def Merge(self, pHead1, pHead2):
        # write code here
        if pHead1 is None:
            return pHead2
        if pHead2 is None:
            return pHead1

        node = sorted_node = ListNode(0)

        while pHead1 and pHead2:
            if pHead1.val < pHead2.val:
                node.next = pHead1
                pHead1 = pHead1.next
            else:
                node.next = pHead2
                pHead2 = pHead2.next
            node = node.next

        if pHead1 or pHead2:
            node.next = pHead1 or pHead2

        return sorted_node.next

    def travel_list(self, node):
        while node is not None:
            print(node.val, end=" ")
            node = node.next
```
**解法二思路：**
使用递归的方法。大体思路同上，先判断临界情况，依次判断输入的两个链表是否为空指针，如果有其中一个为空指针链表，则返回另一个有序的链表。

然后依次比较两个有序链表的值，哪个链表的值较小，则将该节点赋值给新链表。使用递归，直至其中一个链表为None，最终返回和并得到的单调递增链表。
<!--more-->
```
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None

class Solution:
    # 返回合并后列表
    def Merge(self, pHead1, pHead2):
        # write code here
        if pHead1 is None:
            return pHead2
        if pHead2 is None:
            return pHead1

        if pHead1.val < pHead2.val:
            sorted_list = pHead1
            sorted_list.next = self.Merge(pHead1.next, pHead2)
        else:
            sorted_list = pHead2
            sorted_list.next = self.Merge(pHead1, pHead2.next)

        return sorted_list

    def travel_list(self, node):
        while node is not None:
            print(node.val, end=" ")
            node = node.next
```
**代码测试：**

```
if __name__ == "__main__":
    node_1 = ListNode(0)
    node_1.next = ListNode(4)
    node_1.next.next = ListNode(10)
    node_2 = ListNode(2)
    node_2.next = ListNode(3)
    node_2.next.next = ListNode(12)
    solu_1 = Solution()
    # print(solu_1.travel_list(solu_1.Merge(node_1, node_2)))
    solu_2 = Solution2()
    print(solu_2.travel_list(solu_2.Merge(node_1, node_2)))

>>> 0 2 3 4 10 12 None
```

### JZ14 链表中倒数第k个结点
**题目描述：**
> 输入一个链表，输出该链表中倒数第k个结点。

**解法一思路：**

```
lass ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None


class Solution:
    # 时间复杂度： O(n)
    # 空间复杂度： O(n)
    def FindKthToTail(self, head, k):
        # write code here
        res = []

        while head is not None:
            res.insert(0, head.val)
            head = head.next

        if len(res) < k or k < 1:
            return

        return res[k-1]

    def travel_listNode(self, listNode):
        while listNode:
            print(listNode.val, end=" ")
            listNode = listNode.next
```

**解法二思路：**
```
class Solution2:
    # 时间复杂度： O(n)
    # 空间复杂度： O(1)
    def FindKthToTail(self, head, k):
        # write code here
        if k < 0 or head is None:
            return

        slow, fast = head, head
        count = 0
        while fast.next is not None:
            fast = fast.next
            if count >= k - 1:
                slow = slow.next
            count += 1

        if count >= k - 1:
            return slow.val

        return None
```
**代码测试：**

```
if __name__ == "__main__":
    curr = head = ListNode(0)
    head.next = ListNode(1)
    head = head.next
    head.next = ListNode(2)
    head = head.next
    head.next = ListNode(3)
    head = head.next
    head.next = ListNode(4) 

    print(Solution().travel_listNode(curr))
    print(Solution().FindKthToTail(curr, 5))
    print(Solution2().FindKthToTail(curr, 5))

>>> 0 1 2 3 4 None
>>> 0
>>> 0
```
### JZ15 反转链表
**题目描述：**
> 输入一个链表，反转链表后，输出新链表的表头。

**解法思路：**
```
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None


class Solution:
    # 返回ListNode
    def ReverseList(self, pHead):
        # write code here
        if not pHead or pHead.next is None:
            return pHead

        curr, prev = pHead, None
        while curr:
            tmp = curr.next
            curr.next = prev
            prev = curr
            curr = tmp

        return prev
```
**代码测试：**
```
if __name__ == "__main__":
    curr = head = ListNode(0)
    head.next = ListNode(1)
    head = head.next
    head.next = ListNode(2)
    head = head.next
    head.next = ListNode(3)
    head = head.next
    # print(head)
    print(curr)

    res = Solution().ReverseList(curr)
    print(res)
```

### JZ13 从尾到头打印链表
**题目描述：**
> 输入一个链表，按链表从尾到头的顺序返回一个ArrayList。

**解法思路：**
```
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None


class Solution:
    # 返回从尾部到头部的列表值序列，例如[1,2,3]
    def printListFromTailToHead(self, listNode):
        # write code here
        res = []
        while listNode:
            res.insert(0, listNode.val)
            listNode = listNode.next

        return res

    def travel_listNode(self, listNode):
        while listNode:
            print(listNode.val, end=" ")
            listNode = listNode.next
```
**代码测试：**
```
if __name__ == "__main__":
    curr = head = ListNode(0)
    head.next = ListNode(1)
    head = head.next
    head.next = ListNode(2)
    head = head.next
    head.next = ListNode(3)
    head = head.next
    head.next = ListNode(4)

    print(Solution().travel_listNode(curr))
    print(Solution().printListFromTailToHead(curr))
```