title: 剑指offer 第六天
date: 2020-07-13 20:43:55
tags: 剑指offer
category: 刷题
---
### JZ6 旋转数组的最小数字
**题目描述：**
> 把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。
输入一个非递减排序的数组的一个旋转，输出旋转数组的最小元素。
例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1。
NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。

**解法思路：**
这题主要是考察二分查找。**二分查找**，又叫折半查找。它的前提是线性表中的数据必须是有序的，线性表必须采用顺序存储。主要思想是：在有序表中，去中间记录作为比较对象，若给定值与中间记录的关键字相等，则查找成功；若给定值小于中间记录的关键字，则在中间记录的左半区继续查找；若给定值大于中间记录的关键字，则在中间记录的右半区继续查找；不断重复上述过程，直至查找成功，或所有查找区域无记录，查找失败为止。

上面介绍了二分查找的主要思路，而本题难点在于找到中间值与谁进行比较？ 再认真审下题，给定的是非递减排序的数组，也就是“平序”或升序。例如：`[1, 2, 3, 3, 3, 5, 6]`（“平序”），`[1, 2, 3, 4, 5, 6]`（升序）。然后再做旋转得到旋转数组`[3, 3, 5, 6, 1, 2, 3]`，`[4, 5, 6, 1, 2, 3]`，可以确定的是旋转后的数组`nums[0] >= nums[-1]`恒成立。

这样也就得到了`nums[mid]`与哪个值进行比较了，当然是：

* `nums[mid] > nums[left]` , 这个时候 `left = mid + 1`
* `nums[mid] < nums[right]`, 这个时候 `right = mid`
* `nums[mid] = nums[right]`, 这个时候 `left += 1`

这里可以一定意义上认为`nums[left]`与`nums[right]`近似相等，这样便于理解。

> 注: 以上`nums`代表传入的旋转数组，`left`指数组的第一个数，`right`指数组末尾的数，`mid`指数组中间位置的数。
<!--more-->
```
class Solution:
    def minNumberInRotateArray(self, rotateArray):
        # write code here
        if len(rotateArray) == 0:
            return 0
        left = 0
        right = len(rotateArray) - 1
        while left < right:
            if rotateArray[left] < rotateArray[right]:
                return rotateArray[left]

            mid = (left + right) // 2
            if rotateArray[mid] > rotateArray[left]:
                left = mid + 1
            elif rotateArray[mid] < rotateArray[right]:
                right = mid
            else:
                left += 1

        return rotateArray[left]
```
**代码测试：**
```
if __name__ == "__main__":
    print(Solution().minNumberInRotateArray([1, 0, 1, 1, 1]))

>>> 0

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