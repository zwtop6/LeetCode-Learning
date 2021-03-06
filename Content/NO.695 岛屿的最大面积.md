## 题目描述

```
给定一个包含了一些 0 和 1 的非空二维数组 grid 。
一个 岛屿 是由一些相邻的 1 (代表土地) 构成的组合，这里的「相邻」要求两个 1 必须在水平或者竖直方向上相邻。
你可以假设 grid 的四个边缘都被 0（代表水）包围着。
找到给定的二维数组中最大的岛屿面积。(如果没有岛屿，则返回面积为 0 。)

示例 1:

[[0,0,1,0,0,0,0,1,0,0,0,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,1,1,0,1,0,0,0,0,0,0,0,0],
 [0,1,0,0,1,1,0,0,1,0,1,0,0],
 [0,1,0,0,1,1,0,0,1,1,1,0,0],
 [0,0,0,0,0,0,0,0,0,0,1,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,0,0,0,0,0,0,1,1,0,0,0,0]]
对于上面这个给定矩阵应返回 6。注意答案不应该是 11 ，因为岛屿只能包含水平或垂直的四个方向的 1 。

示例 2:

[[0,0,0,0,0,0,0,0]]
对于上面这个给定的矩阵, 返回 0。

注意: 给定的矩阵grid 的长度和宽度都不超过 50。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/max-area-of-island
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

## 我的开始思路

维护一个同样大小的表格来记录是否遍历过，然后按照上下左右的顺寻来不断查找记录

但是，写不出来代码...

## 学习他人的思路

运用深度优先搜索（DFS）和递归的方法

## 代码

- 语言支持：C#

```C#
        private int MaxAreaOfIsland(int[][] grid)
        {
            int result = 0;

            for (int i = 0; i < grid.Length; i++)
            {
                for (int j = 0; j < grid[0].Length; j++)
                {
                    if (grid[i][j] == 1)
                    {
                        int area = 1;

                        area = MaxAreaDFS(grid, i, j, area);
                        if (area > result) result = area;
                    }
                }
            }

            return result;
        }

        private int MaxAreaDFS(int[][] grid, int i, int j, int area)
        {
            //边界条件
            if (i < 0 || i > grid.Length - 1 || j > grid[0].Length - 1 || j < 0) return 0;
            if (grid[i][j] == 0) return 0;

            grid[i][j] = 0;
            area = area + MaxAreaDFS(grid, i + 1, j, area) + MaxAreaDFS(grid, i - 1, j, area) 
                + MaxAreaDFS(grid, i, j + 1, area) + MaxAreaDFS(grid, i, j - 1, area);

            //用传入值做记录，因为是多个递归相加，如果还原现场，则会造成部分数据多次计算
            //grid[i][j] = 1;

            return area;
        }
```

## 学习思路后尝试自己写代码所遇到的问题

- 边界条件反复调试才正确

  边界条件是否考虑全面时这种题目的一大考点，目前没有发现什么好的方法，多练习，多思考吧

- 为什么在此递归中不做还原操作

  因为是多个递归相加，如果还原现场，则会造成部分数据多次计算

## 所学、所悟

- 深度优先遍历一般搭配栈来使用

- 深度优先遍历（DFS）代码模板/思路方向

```
    DFS（）
    {
        1.边界条件
        2.选取候选人/遍历
            2.1 满足条件
    }
 ```
