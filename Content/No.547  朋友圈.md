## 题目描述

```
给定 n 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。 感谢 Marcos 贡献此图。

示例:

输入: [0,1,0,2,1,0,1,3,2,1,2,1]
输出: 6

```

## 我的开始思路

- 思路一

找到所有起作用的边界位置，然后计算这些位置中的存水量

找边界位置的思路是：先找到所有大值（左右两边的数都小于等于该数），然后在不断去掉其中的小值（左右两边的数都大于等于该数）

## 代码

- 语言支持：C#
- 思路二

```C#
        private int Trap1(int[] height)
        {
            int result = 0;
            List<int> list = new List<int>();

            if (height.Length <= 2) return 0;

            //单独处理左边界
            if (height[0] > 0 && height[0] > height[1])
            {
                list.Add(0);
            }

            for (int i = 1; i < height.Length - 1; i++)
            {
                if (height[i] >= height[i - 1] && height[i] >= height[i + 1])
                {
                    list.Add(i);
                }
            }

            //单独处理右边界
            if (height[height.Length - 2] < height[height.Length - 1] && height[height.Length - 1] > 0)
            {
                list.Add(height.Length - 1);
            }

            //移除数后队列发生了变化，不能继续找，可以从头开始找，也可以从移除数的前一个开始找
            for (int i = 1; i < list.Count - 1; i++)
            {
                if (height[list[i]] <= height[list[i - 1]] && height[list[i]] <= height[list[i + 1]])
                {
                    list.RemoveAt(i);
                    i=0;
                }
            }

            if (list.Count < 2) return 0;

            for (int i = 0; i < list.Count - 1; i++)
            {
                int left = list[i];
                int right = list[i + 1];

                int min;
                min = Math.Min(height[left], height[right]);

                for (int j = left + 1; j < right; j++)
                {
                    if (min > height[j]) result += min - height[j];
                }
            }

            return result;
        }
```

## 学习他人的思路

- 双指针、栈

学习中.....
