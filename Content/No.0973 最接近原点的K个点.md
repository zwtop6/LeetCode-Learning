## 题目描述

我们有一个由平面上的点组成的列表 points。需要从中找出 K 个距离原点 (0, 0) 最近的点。

（这里，平面上两点之间的距离是欧几里德距离。）

你可以按任何顺序返回答案。除了点坐标的顺序之外，答案确保是唯一的。

示例 1：
```
输入：points = [[1,3],[-2,2]], K = 1
输出：[[-2,2]]
解释： 
(1, 3) 和原点之间的距离为 sqrt(10)，
(-2, 2) 和原点之间的距离为 sqrt(8)，
由于 sqrt(8) < sqrt(10)，(-2, 2) 离原点更近。
我们只需要距离原点最近的 K = 1 个点，所以答案就是 [[-2,2]]。
```
示例 2：
```
输入：points = [[3,3],[5,-1],[-2,4]], K = 2
输出：[[3,3],[-2,4]]
（答案 [[-2,4],[3,3]] 也会被接受。）
```
提示：
```
1 <= K <= points.length <= 10000
-10000 < points[i][0] < 10000
-10000 < points[i][1] < 10000
```
来源：力扣（LeetCode）  
链接：https://leetcode-cn.com/problems/k-closest-points-to-origin  
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 我的开始思路

不会

## 学习他人的思路

基于快速排序的选择排序

## 代码

- 语言支持：C#

```C#
        public int[][] KClosest(int[][] points, int K)
        {
            Sort(points, 0, points.Length - 1, K);

            int[][] result = new int[K][];

            for (int i = 0; i < K; i++)
            {
                result[i] = points[i];
            }

            return result;

        }

        private void Sort(int[][] points, int left, int right, int K)
        {
            while (left < right)
            {
                int num = partation(points, left, right);

                if (num == K)
                {
                    return;
                }
                else if (num < K)
                {
                    left = num + 1;

                    Sort(points, left, right, K);
                }
                else
                {
                    right = num - 1;

                    Sort(points, left, right, K);
                }
            }
        }

        private int partation(int[][] points, int left, int right)
        {
            int value = Dist(points[right]);
            int cur = left;

            for (int i = left; i < right; i++)
            {
                if (Dist(points[i]) < value)
                {
                    Swap(points, cur, i);
                }
            }

            Swap(points, cur, right);
    
            return cur;
        }

        private void Swap(int[][] points, int i, int j)
        {
            int[] tmp = points[i];

            points[i] = points[j];
            points[j] = tmp;
        }

        private int Dist(int[] point)
        {
            return point[0] * point[0] + point[1] * point[1];
        }
```

## 所学、所悟

基本上，不要求返回值排序+第K个，则都可以用该方法

## 扩展

- 基于第K个的快速选择排序

- 语言支持：C#

```C#
        /// <summary>
        /// 基于选择第K个的快速排序
        /// </summary>
        /// <param name="nums">无序数组</param>
        /// <param name="left">排序左边界</param>
        /// <param name="right">排序有边界</param>
        public void QuickSortsSelectK(int[] nums, int left, int right, int k)
        {
            while (left < right)
            {
                int index = Parition(nums, left, right);

                if (index == k) return;
                else if (index < k)
                {
                    left = index + 1;
                }
                else
                {
                    right = index - 1;
                }
            }
        }

        /// <summary>
        /// 分隔，数组前部分为小于，后部分大于
        /// </summary>
        /// <param name="nums">数组</param>
        /// <param name="left">排序左边界</param>
        /// <param name="right">排序有边界</param>
        /// <returns>排好位置的index</returns>
        private int Parition(int[] nums, int left, int right)
        {
            int value = nums[right];
            int cur = left;

            for (int i = left; i < right; i++)
            {
                if (nums[i] < value)
                {
                    Swap(nums, cur, i);
                    cur++;
                }
            }

            Swap(nums, cur, right);

            return cur;
        }
        
        //交换数组指定位置
        private void Swap(int[] nums, int i, int j)
        {
            if (i == j) return;

            int tmp = nums[i];

            nums[i] = nums[j];
            nums[j] = tmp;
        }
```
