## 题目描述


给定 n 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

![image text](https://github.com/zwtop6/LeetCode-Learning/blob/master/Images/rainwatertrap.png)

上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。 感谢 Marcos 贡献此图。

示例:
```
输入: [0,1,0,2,1,0,1,3,2,1,2,1]
输出: 6
```

## 我的开始思路

找到所有起作用的边界位置，然后计算这些位置中的存水量

先找到所有大值（左右两边的数都小于等于该数），然后在不断去掉其中的小值（左右两边的数都大于等于该数）

## 代码

- 语言支持：C#

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

- 按行找

每行的存水量是由该行左边最大值和右边最大值决定的，所以遍历每行，找到该行左右两边的最值可求

找最值可以通过记录的方式来减少遍历次数

- 从结果数组倒推

当接满雨水后，数组变得更加规律

在某一个最大值的左边不严格单调递增，右边不严格单调递减

因为有一个最大值，所以左边的边界高度是由该数左边的值决定，右边同理

那么只需要从左向右遍历到最大值，把数组补成不严格单调增，右边同理

时间复杂度是遍历了两遍数组O(N),空间复杂度为几个变量O(1)

- 单调栈

勉强理解了，但是没有消化掉

再碰到运用单调栈的题回过头来一起看

## 代码

- 语言支持：C#
- 思路：从结果数组倒推

```
private int Trap2(int[] height)
        {
            int result = 0;
            int max = 0;
            int maxIndex = 0;

            if (height.Length == 0) return 0;

            //找最大值的index
            for (int i = 1; i < height.Length; i++)
            {
                if (height[i] > max)
                {
                    max = height[i];
                    maxIndex = i;
                }
            }

            int curMax = height[0];

            //从左往右找到最大值处
            for (int i = 1; i < maxIndex; i++)
            {
                if (height[i] < curMax)
                {
                    result += curMax - height[i];
                }
                else
                {
                    curMax = height[i];
                }
            }

            curMax = height[height.Length - 1];
            //从右往左找到最大值处
            for (int i = height.Length - 2; i > maxIndex; i--)
            {
                if (height[i] < curMax)
                {
                    result += curMax - height[i];
                }
                else
                {
                    curMax = height[i];
                }
            }

            return result;
        }
```        

## 所学、所悟
