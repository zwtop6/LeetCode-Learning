## 题目描述

```
班上有 N 名学生。其中有些人是朋友，有些则不是。他们的友谊具有是传递性。如果已知 A 是 B 的朋友，B 是 C 的朋友，那么我们  
可以认为 A 也是 C 的朋友。所谓的朋友圈，是指所有朋友的集合。

给定一个 N * N 的矩阵 M，表示班级中学生之间的朋友关系。如果M[i][j] = 1，表示已知第 i 个和 j 个学生互为朋友关系，否则为  
不知道。你必须输出所有学生中的已知的朋友圈总数。

示例 1:

输入: 
[[1,1,0],
 [1,1,0],
 [0,0,1]]
输出: 2 

说明：已知学生0和学生1互为朋友，他们在一个朋友圈。
第2个学生自己在一个朋友圈。所以返回2。

示例 2:

输入: 
[[1,1,0],
 [1,1,1],
 [0,1,1]]
输出: 1

说明：已知学生0和学生1互为朋友，学生1和学生2互为朋友，所以学生0和学生2也是朋友，所以他们三个在一个朋友圈，返回1。
注意：

N 在[1,200]的范围内。
对于所有学生，有M[i][i] = 1。
如果有M[i][j] = 1，则有M[j][i] = 1。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/max-area-of-island
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

## 我的开始思路

- 思路一

想到了深度优先遍历，广度优先遍历

- 思路二

还想到了用数组来维护，数组长度为学生个数，数组内容相当于记录该学生属于第几个朋友圈，

遍历半个矩阵，当数字为1时，维护数组，有三种情况

1. 如果两个人的标记都为0，则直接标记为新的一个朋友圈

2. 如果一个人是0而另一个不是，则把标记为0的位置改为另一个人的朋友圈数

3. 两个人都不是0，则把数组中所有等于这两个人中的大数改为小数，合并成同一个朋友圈

最后看一下数组中有多少个不同的数，即有多少个朋友圈，即为答案

## 代码

- 语言支持：C#
- 思路二

```C#
        private int FindCircleNum1(int[][] M)
        {
            int result = 0;

            int[] tag = new int[M.Length];

            for (int i = 0; i < tag.Length; i++)
            {
                tag[i] = 0;
            }

            for (int i = 0; i < M.Length; i++)
            {
                for (int j = i; j < M[0].Length; j++)
                {
                    if (M[i][j] == 1)
                    {
                        
                        if (tag[i] == 0 && tag[j] == 0)
                        {
                            //不属于任何朋友圈，赋值为新朋友圈
                            tag[i] = tag.Max() + 1;
                            tag[j] = tag[i];
                        }
                        else if (tag[i] != 0 && tag[j] == 0)
                        {
                            //把不为0的放入为0的人的朋友圈
                            tag[j] = tag[i];
                        }
                        else if (tag[i] == 0 && tag[j] != 0)
                        {
                            //把不为0的放入为0的人的朋友圈
                            tag[i] = tag[j];
                        }
                        else
                        {
                            //把大号朋友圈的全部归为小号朋友圈
                            if (tag[i] < tag[j])
                            {
                                int nums = tag[j];

                                for (int m = 0; m < tag.Length; m++)
                                {
                                    if (tag[m] == nums) tag[m] = tag[i];
                                }
                            }
                            if (tag[i] > tag[j])
                            {
                                int nums = tag[i];

                                for (int m = 0; m < tag.Length; m++)
                                {
                                    if (tag[m] == nums) tag[m] = tag[j];
                                }
                            }
                        }
                    }
                }
            }


            for (int i = 1; i <= tag.Length; i++)
            {
                if (tag.Contains(i))
                {
                    result++;
                    continue;
                }
            }

            return result;
        }
```

- 语言支持：C#
- DFS

```C#
        private int FindCircleNum2(int[][] M)
        {
            int result = 0;
            int[] tag = new int[M.Length];

            for (int i = 0; i < M.Length; i++)
            {
                if (tag[i] == 0)
                {
                    tag[i] = 1;
                    FindCircleNumDFS(M, tag, i);
                    result++;
                }
            }

            return result;
        }

        private void FindCircleNumDFS(int[][] M, int[] tag, int i)
        {
            //不能从i+1开始找，因为比如0和3是朋友，找3的朋友时可能会漏掉1和2
            for (int j = 0; j < M.Length; j++)
            {
                if (M[i][j] == 1 && tag[j] == 0)
                {
                    tag[j] = 1;
                    FindCircleNumDFS(M, tag, j);
                }
            }
        }
```

- 语言支持：C#
- BFS

```C#
        private int FindCircleNum3(int[][] M)
        {
            int result = 0;
            int[] tag = new int[M.Length];
            Queue<int> queue = new Queue<int>();

            for (int i = 0; i < M.Length; i++)
            {
                if (tag[i] == 0)
                {
                    tag[i] = 1;
                    queue.Enqueue(i);

                    while (queue.Count != 0)
                    {
                        int x = queue.Dequeue();

                        for (int j = 0; j < M.Length; j++)
                        {
                            if (M[x][j] == 1 && tag[j] == 0)
                            {
                                tag[j] = 1;
                                queue.Enqueue(j);
                            }
                        }
                    }
                    result++;
                }
            }
            return result;
        }
```

## 学习他人的思路

- 并查集

## 代码

- 语言支持：C#

```
private int FindCircleNum4(int[][] M)
        {
            int result = M.Length;

            int[] tag = new int[M.Length];

            for (int i = 0; i < tag.Length; i++)
            {
                tag[i] = i;
            }

            for (int i = 0; i < M.Length; i++)
            {
                for (int j = i; j < M[0].Length; j++)
                {
                    if (M[i][j] == 1)
                    {
                        result = Union(tag, i, j, result);
                    }
                }
            }

            return result;
        }

        private int Union(int[] M, int x, int y, int count)
        {
            int rootX = FindRoot(M, x);
            int rootY = FindRoot(M, y);

            if (rootX != rootY)
            {
                M[rootX] = M[rootY];
                count--;
            }

            return count;
        }

        private int FindRoot(int[] M, int x)
        {
            while (M[x] != x)
            {
                M[x] = M[M[x]];
                x = M[x];
            }

            return x;
        }
```

## 所学、所悟

我自己用数组维护的思路，有一些并查集的味道

主要相同点

  1. 再遍历的过程中随着情况的改变而不断的修正维护的数组，不断的把相同的放在一起

不同点/可优化

  1. 我的没有在遍历过程中去记录实时的总集数，导致要在最后查询一边来得到最终集合数    
  1. 我的数组只是记录自己在哪个集合中，没有父节点的记录，导致了当有不同两个集合需要合并时，就得遍历一次数组

并查集里的压缩路径，要视情况是否使用，如果需要数据之间的父子关系，则不压缩，但是数据量很大的话，不压缩导致查找两个是否  
属于同一集合会很耗时，那么实际情况可能要维护两个数组来对应不同的情况使用，用空间换时间在大部分时候都是划算的。
