title: 剑指offer 第四天
tags: 剑指offer
category: 刷题
date: 2020-07-11 23:10:27
---
### JZ4 重建二叉树
**题目描述：**
> 输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列 [1,2,4,7,3,5,6,8] 和中序遍历序列 [4,7,2,1,5,3,8,6] ，则重建二叉树并返回。

**解法思路：**
使用递归的方法求解。这道题需要对二叉树的性质比较了解，需要熟悉二叉树的几种遍历方式。

**前序遍历是先遍历根节点，然后遍历左节点，最后遍历右节点（根左右）**

**中序遍历是先遍历左节点，然后遍历根节点，最后遍历右节点（左根右）**

**后序遍历是先遍历左节点，然后遍历右节点，最后遍历根节点（根左右）**

本题给定前序遍历以及中序遍历的序列，可以知道前序遍历第一个值一定是根节点。而根节点在中序遍历中会把左右子树分成两半。因此本题的关键是找到根节点，然后将左右子树依次递归重建得到二叉树。

使用递归是需要结束条件的，当前序或中序遍历序列为`None`时，便可推出递归，得到二叉树。
<!--more-->
```
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None


class Solution:
    # 返回构造的TreeNode根节点
    def reConstructBinaryTree(self, pre, tin):
        if not pre or not tin:
            return None

        root = TreeNode(pre.pop(0))
        index = tin.index(root.val)

        root.left = self.reConstructBinaryTree(pre, tin[:index])
        root.right = self.reConstructBinaryTree(pre, tin[index + 1:])

        return root

    def preOrder(self, root):
        if root is None:
            return

        print(root.val, end=' ')
        self.preOrder(root.left)
        self.preOrder(root.right)

    def InOrder(self, root):
        if root is None:
            return
        self.InOrder(root.left)
        print(root.val, end=' ')
        self.InOrder(root.right)

    def BackOrder(self, root):
        if root is None:
            return
        self.BackOrder(root.left)
        self.BackOrder(root.right)
        print(root.val, end=' ')
```
本题只需写好`reConstructBinaryTree`这个方法，上面代码其他三个函数是用来前序、中序以及后序遍历重建得到二叉树的，以方便大家调试。
**代码测试：**
```
if __name__ == "__main__":
    pre = [1, 2, 4, 7, 3, 5, 6, 8]
    tin = [4, 7, 2, 1, 5, 3, 8, 6]

    reTree = Solution().reConstructBinaryTree(pre, tin)
    print("前序遍历：")
    Solution().preOrder(reTree)
    print("\n")
    print("中序遍历：")
    Solution().InOrder(reTree)
    print("\n")
    print("后序遍历：")
    Solution().BackOrder(reTree)

>>> 前序遍历：
1 2 4 7 3 5 6 8 

中序遍历：
4 7 2 1 5 3 8 6 

后序遍历：
7 4 2 5 8 6 3 1 

```

### JZ18 二叉树的镜像
**题目描述：**
> 操作给定的二叉树，将其变换为源二叉树的镜像。
 二叉树的镜像定义如下：
  
         源二叉树
            8
           /  \
          6   10
         / \  / \
        5  7 9 11

        镜像二叉树
             8
           /  \
          10   6
         / \  / \
        11 9 7  5

**解法思路：**

```
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None


class Solution:
    # 返回镜像树的根节点
    def Mirror(self, root):
        # write code here
        if root is None:
            return

        root.left, root.right = \
            self.Mirror(root.right), self.Mirror(root.left)

        return root

    def preOrder(self, root):
        if root is None:
            return

        print(root.val, end=' ')
        self.preOrder(root.left)
        self.preOrder(root.right)
```
**代码测试：**
```
if __name__ == "__main__":
    tree1 = TreeNode(8)
    tree1.left = TreeNode(6)
    tree1.right = TreeNode(10)
    tree1.left.left = TreeNode(5)
    tree1.left.right = TreeNode(7)
    tree1.right.left = TreeNode(9)
    tree1.right.right = TreeNode(11)

    invert_tree = Solution().Mirror(tree1)
    print(Solution().preOrder(invert_tree))

>>> 8 10 11 9 6 7 5 None
```
### JZ23 二叉搜索树的后序遍历
**题目描述：**
> 输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历的结果。如果是则输出Yes,否则输出No。假设输入的数组的任意两个数字都互不相同。

**解法思路：**
```
class Solution:
    def VerifySquenceOfBST(self, sequence):
        # write code here
        if len(sequence) == 0:
            return False

        root = sequence[-1]
        for index, value in enumerate(sequence):
            if value > root:
                break

        for value in sequence[index: -1]:
            if value < root:
                return False

        left = True
        if index > 0:
            left = self.VerifySquenceOfBST(sequence[:index])

        right = True
        if index < len(sequence) - 1:
            right = self.VerifySquenceOfBST(sequence[index: -1])

        return left and right
```
**代码测试：**
```
if __name__ == "__main__":
    is_binary_tree = [1, 3, 5, 9, 12, 10, 7]
    print(Solution().VerifySquenceOfBST(is_binary_tree))

>>> True
```

### JZ67 剪绳子
**题目描述：**
>给你一根长度为n的绳子，请把绳子剪成整数长的m段（m、n都是整数，n>1并且m>1，m<=n），每段绳子的长度记为 k[1],...,k[m]。请问 k[1]x...xk[m] 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

> 输入：8 
输出：18 

**解法思路：**

**代码测试：**