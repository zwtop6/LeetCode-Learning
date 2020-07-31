## 题目描述

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”

例如，给定如下二叉树:  root = [3,5,1,6,2,0,8,null,null,7,4]

![image.png]()

示例 1:
```
输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
输出: 3
解释: 节点 5 和节点 1 的最近公共祖先是节点 3。
```

示例 2:
```
输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
输出: 5
解释: 节点 5 和节点 4 的最近公共祖先是节点 5。因为根据定义最近公共祖先节点可以为节点本身。
```
 
说明:

所有节点的值都是唯一的。
p、q 为不同节点且均存在于给定的二叉树中。

来源：力扣（LeetCode）  
链接：https://leetcode-cn.com/problems/reverse-linked-list  
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 我的开始思路

利用DFS可以得到两节点的所有父节点栈，然后比较两个栈中的元素，找到相同的

因为是共同祖先，所以数量少的栈里一定有祖先，所以数量多的栈相对于少的栈多出来的部分则一定没有祖先，直接移除

移除后两个栈数量相等，按序比较即可

## 代码

- 语言支持：C#

```C#
        public TreeNode LowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q)
        {
            Stack<TreeNode> stackP = new Stack<TreeNode>();
            Stack<TreeNode> stackQ = new Stack<TreeNode>();

            bool isGoon = true;
            LowestCommonAncestorDFS(root, p, stackP, ref isGoon);

            isGoon = true;
            LowestCommonAncestorDFS(root, q, stackQ, ref isGoon);

            if (stackP.Count > stackQ.Count)
            {
                while (stackP.Count != stackQ.Count)
                {
                    stackP.Pop();
                }
            }
            else
            {
                while (stackP.Count != stackQ.Count)
                {
                    stackQ.Pop();
                }
            }

            while (stackP.Peek().val != stackQ.Peek().val)
            {
                stackQ.Pop();
                stackP.Pop();
            }

            return stackP.Pop();
        }
        
        //isGoon表明是否需要继续查找
        private void LowestCommonAncestorDFS(TreeNode root, TreeNode aim, Stack<TreeNode> result, ref bool isGoon)
        {
            if (root == null || isGoon == false) return;
            if (root.val == aim.val)
            {
                result.Push(root);
                isGoon = false;
                return;
            }

            result.Push(root);
            LowestCommonAncestorDFS(root.left, aim, result, ref isGoon);

            if (isGoon)
            {
                LowestCommonAncestorDFS(root.right, aim, result, ref isGoon);

                if (isGoon)
                {
                    result.Pop();
                }
                else return;
            }
            else
            {
                return;
            }
        }
```

## 学习他人的思路

直接不断递归自己

如果两个节点在某节点的两边，则该节点为最近祖先

如果某节点在另一节点的根节点的另一边没有，则一定在该节点的下面，那么该节点为最近祖先

## 代码

- 语言支持：C#

```C#
        public TreeNode LowestCommonAncestor2(TreeNode root, TreeNode p, TreeNode q)
        {
            if (root == null) return null;
            if (root.val == p.val || root.val == q.val) return root;

            TreeNode leftlow = LowestCommonAncestor2(root.left, p, q);
            TreeNode rightlow = LowestCommonAncestor2(root.right, p, q);
            
            //有空，表明都在同一遍
            if (leftlow == null) return rightlow;
            if (rightlow == null) return leftlow;
            
            //都不空，表明在两边
            return root;
        }
```

## 所学、所悟

把问题规模先缩减到尽量小，然后进行分析，在扩充

比如该题，先想只有两层的情况

1.在两边，则最近祖先为根节点  
2.根节点和另一个，则最近祖先为根节点  

那么就有可能发现：

1.如果两个节点在某节点的两边，则该节点为最近祖先  
2.如果某节点在另一节点的根节点的另一边没有，则一定在该节点的下面，那么该节点为最近祖先
