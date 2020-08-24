## 题目描述

给定一个二叉搜索树的根节点 root 和一个值 key，删除二叉搜索树中的 key 对应的节点，并保证二叉搜索树的性质不变。返回二叉搜索树（有可能被更新）的根节点的引用。

一般来说，删除节点可分为两个步骤：

首先找到需要删除的节点；  
如果找到了，删除它。  
说明： 要求算法时间复杂度为 O(h)，h 为树的高度。

示例:
```
root = [5,3,6,2,4,null,7]
key = 3

    5
   / \
  3   6
 / \   \
2   4   7

给定需要删除的节点值是 3，所以我们首先找到 3 这个节点，然后删除它。

一个正确的答案是 [5,4,6,2,null,null,7], 如下图所示。

    5
   / \
  4   6
 /     \
2       7

另一个正确答案是 [5,2,6,null,4,null,7]。

    5
   / \
  2   6
   \   \
    4   7
```
来源：力扣（LeetCode）  
链接：https://leetcode-cn.com/problems/delete-node-in-a-bst  
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 我的开始思路

首先找到给定值，如果当前结点大于key，则在左子树找，如果小于，则在右子树找

找到后，继续找该结点的左子树中最大的值或者右子树中最小的值

把找到的页子结点增加到key结点处，删除key结点

然偶写代码后发现还要记住key结点的前一个结点和找到页子结点的前一个结点

相当于又增加了结点的插入，变得更加复杂了

## 学习他人的思路

首先都是用递归

当当前结点大于Key时，当前结点的左子树=左子树调用函数

当当前结点小于Key时，当前结点的右子树=右子树调用函数

当当前结点等于Key时

- 法一

1. 左子树为空时，返回右子树（包含了左右子树都为空的情况）

1. 右子树为空时，返回左子树

1. 左右子树都不为空时，把左子树放到右子树中最小结点的左子树，然后返回右子树

- 法二

1. 左右子树为空时，返回null

1. 有右子树时，把Key结点的值变为右子树的最小值，返回this(右子树，最小值)

1. 没有右子树时，把Key结点的值变为左子树的最大值，返回this(左子树，最大值)

## 代码

- 语言支持：C#  
- 法一

```C#
         public TreeNode DeleteNode(TreeNode root, int key)
        {
            if (root == null) return root;

            if (root.val > key)
            {
                root.left = DeleteNode(root.left, key);
            }
            else if (root.val < key)
            {
                root.right = DeleteNode(root.right, key);
            }
            else
            {
                if (root.left == null)
                {
                    return root.right;
                }
                else if (root.right == null)
                {
                    return root.left;
                }
                else
                {
                    var tmp = root.right;

                    while (tmp.left != null)
                    {
                        tmp = tmp.left;
                    }

                    tmp.left = root.left;
                    root.left = null;
                    return root.right;
                }
            }

            return root;
        }
```

- 语言支持：C#  
- 法二

```C#
        public TreeNode DeleteNode1(TreeNode root, int key)
        {
            if (root == null) return root;

            if (root.val > key)
            {
                root.left = DeleteNode(root.left, key);
            }
            else if (root.val < key)
            {
                root.right = DeleteNode(root.right, key);
            }
            else
            {
                if (root.left == null && root.right == null)
                {
                    return null;
                }
                else if (root.right != null)
                {
                    //找后继
                    var tmp = root.right;
                    while (tmp.left != null)
                    {
                        tmp = tmp.left;
                    }

                    root.val = tmp.val;
                    root.right = DeleteNode(root.right, tmp.val);
                }
                else
                {
                    //找前驱
                    var tmp = root.left;
                    while (tmp.right != null)
                    {
                        tmp = tmp.right;
                    }

                    root.val = tmp.val;
                    root.left = DeleteNode(root.left, tmp.val);
                }
            }

            return root;
        }
```

## 所学、所悟

- 学会二叉搜索树的删除操作

- 转变不想分类做的思想

每次想到要分类时就不想往下做了，总觉得有不用分类的方法

先不说有没有不用分类的方法，就算有也不容易一下子想到，干想时不行的

要先把类分出来，并且一开始不用考虑最少的分类方法，仅仅去分就好了

分好后再看是否可以有合并的来简化代码或者思想

- 对递归的理解(引用)

**不去管函数的内部细节是如何处理的，我们只看其函数作用以及输入与输出**

对于函数deleteNode来说：
```
函数作用：删除搜索二叉树中的key对应的节点，并保证二叉搜索树的性质不变

输入：二叉搜索树的根节点root和一个值key

输出：新二叉搜索的根节点的引用
```
