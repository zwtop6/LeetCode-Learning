## 题目描述

回与给定先序遍历 preorder 相匹配的二叉搜索树（binary search tree）的根结点。

(回想一下，二叉搜索树是二叉树的一种，其每个节点都满足以下规则，对于 node.left 的任何后代，值总 < node.val，而 node.right 的任何后代，值总 > node.val。此外，先序遍历首先显示节点的值，然后遍历 node.left，接着遍历 node.right。）

示例：
```
输入：[8,5,1,7,10,12]
输出：[8,5,10,1,7,null,12]
```
提示：
```
1 <= preorder.length <= 100
先序 preorder 中的值是不同的。
```
来源：力扣（LeetCode）  
链接：https://leetcode-cn.com/problems/construct-binary-search-tree-from-preorder-traversal  
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 我的开始思路

- 按照给定先序数组找到应放入的数位置，构建树

- 递归，不断的根据第一个数来分解数组，把小于的递归并赋值给左子树，大于的递归并赋值给右子树

## 代码

- 语言支持：C#
- 找位置构建树

```C#
        public TreeNode BstFromPreorder1(int[] preorder)
        {
            if (preorder.Length == 0) return null;

            TreeNode root = new TreeNode(preorder[0], null, null);

            for (int i = 1; i < preorder.Length; i++)
            {
                var parent = root;
                var cur = root;
                bool isLeft = true;

                //找到应该所处的位置
                while (cur != null)
                {
                    if (preorder[i] < cur.val)
                    {
                        isLeft = true;
                        parent = cur;
                        cur = cur.left;
                    }
                    else
                    {
                        isLeft = false;
                        parent = cur;
                        cur = cur.right;
                    }
                }

                if (isLeft) parent.left = new TreeNode(preorder[i], null, null);
                else parent.right = new TreeNode(preorder[i], null, null);
            }

            return root;
        }
```

- 语言支持：C#
- 递归

```C#
        public TreeNode BstFromPreorder(int[] preorder)
        {

            if (preorder.Length == 0) return null;

            var root = new TreeNode(preorder[0], null, null);

            if (preorder.Length == 1) return root;
            else
            {
                int[] leftList = preorder.Where(c => c < preorder[0]).ToArray();
                int[] rightList = preorder.Where(c => c > preorder[0]).ToArray();

                root.left = BstFromPreorder(leftList);
                root.right = BstFromPreorder(rightList);
            }

            return root;
        }
```
