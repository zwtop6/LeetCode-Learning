## 题目描述

给定 n 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。

求在该柱状图中，能够勾勒出来的矩形的最大面积。


以上是柱状图的示例，其中每个柱子的宽度为 1，给定的高度为 [2,1,5,6,2,3]。

图中阴影部分为所能勾勒出的最大矩形面积，其面积为 10 个单位。

 

示例:
```
输入: [2,1,5,6,2,3]
输出: 10
```

来源：力扣（LeetCode）  
链接：https://leetcode-cn.com/problems/largest-rectangle-in-histogram  
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 我的开始思路

按照高度来找对应的最大宽度

从左往右遍历，分别向左右两边查找第一个小于该高度的位置，此时两位置的差即为宽，高为遍历的数的值，可求面积

时间复杂度O(N^2)，空间复杂度O(1)，不出所料，会超时

## 代码

- 语言支持：C#

```C#
      public int LargestRectangleArea(int[] heights) 
      {
            int max = 0;
            int left;
            int right;

            for (int i = 0; i < heights.Length; i++)
            {
                left = i;
                right = i;

                while (left >= 0)
                {
                    if (heights[left] >= heights[i]) left--;
                    else break;
                }

                while (right <= heights.Length - 1)
                {
                    if (heights[right] >= heights[i]) right++;
                    else break;
                }

                max = Math.Max(max, heights[i] * (right - left - 1));
            }

            return max;
    }
```

## 学习他人的思路

- 单调栈

## 代码

- 语言支持：C#

```C#
     public int LargestRectangleArea(int[] heights) 
     {

        int area=0;
        int[] newHeights=new int[heights.Length+2];
        
        //运用库自己写快很多
        Array.Copy(heights, 0, newHeights, 1, heights.Length);

        Stack<int> stack = new Stack<int>();
        stack.Push(0);

        for (int i=1;i<newHeights.Length;i++)
        {
            if (newHeights[i]>=newHeights[stack.Peek()])
            {
                stack.Push(i);
            }
            else
            {
                int tmp = newHeights[stack.Pop()]*(i-stack.Peek()-1);
                area=Math.Max(area,tmp);
                i--;
            }
        }

        return area;
    }
```

## 所学、所悟

这是在做心动网络笔试题时的最后一题，当时只想到了暴力解法，只能通过50%

想了很久，想不出如何优化

看到这道题时，第一印象是接雨水那道题，但是又想不到共通点

下来学习时，发现是因为接雨水那道题的常规解法“单调栈”当时并不是太理解，而且给放过去了

造成了笔试时的困扰

所以要引以为戒

1. 不要放过知识点

1. 要更注重一般的解法，不要本末倒置

一定要在理解了一般，或者说更广泛可用的解法后，在去关注巧妙的解法，试图理解为什么可以巧妙，是否可以用在其他处

## 扩展
