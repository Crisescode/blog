title: 剑指offer 第七天
tags: 剑指offer
category: 刷题
date: 2020-07-19 21:33:15
---
### JZ19 顺时针打印矩阵
**题目描述：**
> 输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字，例如，如果输入如下4 X 4矩阵： 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 则依次打印出数字1,2,3,4,8,12,16,15,14,13,9,5,6,7,11,10.

**解法思路：**
这道题刚开始自己一看，就是想着设置一些索引位置，按照索引位置打印来打印不就行了。后来再认真读题，发现它输入的二维矩阵大小是变化的，不是固定大小，才发现自己的想法太`native`。

看了下讨论区，发现只是需要确定几个固定位置的索引值，便可以不管输入矩阵的大小，自适应的便能够确定顺时针打印每个位置的索引，然后再根据这个索引来打印矩阵就行，还真是巧妙。其实在确定好几个顶点的位置，也就相当于确定了方向，然后不管是顺时针还是逆时针，下次都可以方便的解题了。


<!--more-->
```
class Solution:
    # matrix类型为二维列表，需要返回列表
    def printMatrix(self, matrix):
        # write code here
        top = 0
        bottom = len(matrix)
        left = 0
        right = len(matrix[0])

        res = []
        # 判断是否越界
        while top < bottom and left < right:
            # 最上面一行，需向右打印
            res.extend([matrix[top][c] for c in range(left, right)])
            # 最右边一行，需向下打印
            res.extend([matrix[r][right - 1] for r in range(top + 1, bottom)])
            # 最下面一行，需向左打印
            if bottom - top > 1:
                res.extend([matrix[bottom - 1][c] for c in range(right - 2, left, -1)]) # 注意需要将右边那个值去除掉
            # 最左边一行，需向上打印
            if right - left > 1:
                res.extend([matrix[r][left] for r in range(bottom - 1, top, -1)])

            top += 1
            bottom -= 1
            left += 1
            right -= 1

        return res
```
**代码测试：**
```
if __name__ == "__main__":
    num = 1
    matrix = [[0 for i in range(4)] for j in range(4)]
    for i in range(4):
        for j in range(4):
            matrix[i][j] = num
            num += 1

    print(Solution().printMatrix(matrix))
    print(Solution().printMatrix([[1, 2], [3, 4], [5, 6], [7, 8], [9, 10]]))

>>> [1, 2, 3, 4, 8, 12, 16, 15, 14, 13, 9, 5, 6, 7, 11, 10]
[1, 2, 4, 6, 8, 10, 9, 7, 5, 3]


```

### JZ38 二叉树的深度
**题目描述：**
> 输入一棵二叉树，求该树的深度。从根结点到叶结点依次经过的结点（含根、叶结点）形成树的一条路径，最长路径的长度为树的深度。

**解法思路：**

```
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None


class Solution:
    def TreeDepth(self, pRoot):
        # write code here
        if not pRoot:
            return 0

        x = pRoot

        left = self.TreeDepth(pRoot.left)
        right = self.TreeDepth(pRoot.right)

        return max(left, right) + 1


class Solution2:
    def TreeDepth(self, pRoot):
        # write code here
        if pRoot is None:
            return 0
        q = []
        depth = 0
        q.append(pRoot)
        while len(q):  # 队列为空时说明没有下一层
            length = len(q)
            for i in range(length):  # 遍历层的每个节点看是否有子节点有则加入
                current = q.pop(0)  # current为当前遍历到的层中节点，取出，注意pop(-1)为默认，这里要pop(0),取出第一个，先入先出
                if current.left:
                    q.append(current.left)
                if current.right:
                    q.append(current.right)
            depth += 1
        return depth
```
**代码测试：**
```
if __name__ == "__main__":
    root = TreeNode(0)
    root.left = TreeNode(1)
    root.right = TreeNode(2)
    root.left.left = TreeNode(3)
    root.left.right = TreeNode(4)
    root.right.left = TreeNode(5)
    root.right.left.left = TreeNode(6)

    print(Solution().TreeDepth(root))
    print(Solution2().TreeDepth(root))
    
>>> 4
4
```
### JZ60 把二叉树打印成多行
**题目描述：**
> 从上到下按层打印二叉树，同一层结点从左至右输出。每一层输出一行。

**解法思路：**
```
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None


class Solution:
    # 返回二维列表[[1,2],[4,5]]
    def Print(self, pRoot):
        # write code here
        if not pRoot:
            return []

        node_queue = [pRoot]
        res = []
        while node_queue:
            val = []
            length = len(node_queue)
            for i in range(length):
                current = node_queue.pop(0)
                val.append(current.val)

                if current.left:
                    node_queue.append(current.left)
                if current.right:
                    node_queue.append(current.right)

            res.append(val)

        return res
```
**代码测试：**
```
if __name__ == "__main__":
    root = TreeNode(8)
    root.left = TreeNode(6)
    root.right = TreeNode(10)
    root.left.left = TreeNode(5)
    root.left.right = TreeNode(7)
    root.right.left = TreeNode(9)
    root.right.right = TreeNode(11)

    print(Solution().Print(root))

>>> [[8], [6, 10], [5, 7, 9, 11]]
```