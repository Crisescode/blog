title: 剑指offer 第八天
tags: 剑指offer
category: 刷题
date: 2020-07-20 23:43:10
---
### JZ59 按之字形顺序打印二叉树
**题目描述：**
> 请实现一个函数按照之字形打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右至左的顺序打印，第三行按照从左到右的顺序打印，其他行以此类推。

**解法思路：**
这道题其实是昨天最后一道题[`JZ60`](http://localhost:4000/blog/2020/07/19/%E5%89%91%E6%8C%87offer-%E7%AC%AC%E4%B8%83%E5%A4%A9/)的升级版，其实也就是层次遍历二叉树（BFS）。层次遍历思想可以参见上一道题，只是此题需要结合二叉树的深度`depth`进行解题。在每次需要打印的地方，当深度`depth`对2取余，为0则从左到右打印，为1则反转打印。

```
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None


class Solution:
    def Print(self, pRoot):
        # write code here
        if pRoot is None:
            return []

        depth = 0
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

            if depth % 2 == 0:
                res.append(val)
            else:
                res.append(list(reversed(val)))

            depth += 1

        return res
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

    print(Solution().Print(root))

>>> [[0], [2, 1], [3, 4, 5], [6]]
```
<!--more-->
### JZ58 对称的二叉树
**题目描述：**
> 请实现一个函数，用来判断一棵二叉树是不是对称的。注意，如果一个二叉树同此二叉树的镜像是同样的，定义其为对称的。

**解法思路：**

<!--more-->
```
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None
        
        
class Solution:
    def isSymmetrical(self, pRoot):
        # write code here
        pass
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

### JZ38 二叉树的下一个结点
**题目描述：**
> 给定一个二叉树和其中的一个结点，请找出中序遍历顺序的下一个结点并且返回。注意，树中的结点不仅包含左右子结点，同时包含指向父结点的指针。

**解法思路：**

```
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None


class Solution:
    def GetNext(self, pNode):
        # write code here
        pass
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
### JZ38 平衡二叉树
**题目描述：**
> 输入一棵二叉树，判断该二叉树是否是平衡二叉树。

> 在这里，我们只需要考虑其平衡性，不需要考虑其是不是排序二叉树

**解法思路：**
```
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None


class Solution:
    def IsBalanced_Solution(self, pRoot):
        # write code here
        pass

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