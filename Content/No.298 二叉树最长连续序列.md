## 题目描述

给你一棵指定的二叉树，请你计算它最长连续序列路径的长度。

该路径，可以是从某个初始结点到树中任意结点，通过「父 - 子」关系连接而产生的任意路径。

这个最长连续的路径，必须从父结点到子结点，反过来是不可以的。

示例 1：
```
输入:

   1
    \
     3
    / \
   2   4
        \
         5

输出: 3

解析: 当中，最长连续序列是 3-4-5，所以返回结果为 3
```
示例 2：
```
输入:

   2
    \
     3
    / 
   2    
  / 
 1

输出: 2 

解析: 当中，最长连续序列是 2-3。注意，不是 3-2-1，所以返回 2。
```
作者：力扣 (LeetCode)  
链接：https://leetcode-cn.com/leetbook/read/algorithm-and-interview-skills/xthox6/  
来源：力扣（LeetCode）  
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

## 我的开始思路

递归遍历，同时做判断并记录

## 代码

- 语言支持：C#

```C#
        int max = 0;

        public int LongestConsecutive(TreeNode root)
        {
            if (root == null) return 0;

            DFS(root, root.val, 0);

            return max;
        }

        public void DFS(TreeNode root, int last, int length)
        {
            if (root == null) return;
            else
            {
                if (root.val == last + 1)
                {
                    length++;
                    if (length > max) max = length;
                }
                else
                {
                    length = 1;
                    if (length > max) max = length;
                }

                DFS(root.left, root.val, length);
                DFS(root.right, root.val, length);
            }
        }
```
