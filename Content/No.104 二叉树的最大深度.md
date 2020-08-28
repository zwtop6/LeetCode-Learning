## 题目描述

给定一个 N 叉树，找到其最大深度。

最大深度是指从根节点到最远叶子节点的最长路径上的节点总数。

说明:

树的深度不会超过 1000。  
树的节点总不会超过 5000。

来源：力扣（LeetCode）  
链接：https://leetcode-cn.com/problems/maximum-depth-of-n-ary-tree  
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 我的开始思路

递归

## 代码

- 语言支持：C#

```C#
        public int MaxDepth(Node root)
        {

            if (root == null) return 0;
            if (root.children == null) return 1;

            int max = 0;

            for (int i = 0; i < root.children.Count; i++)
            {
                int tmpMax = MaxDepth(root.children[i]);

                if (tmpMax > max)
                {
                    max = tmpMax;
                }
            }

            return max + 1;
        }
```
