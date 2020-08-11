## 题目描述

给定一个含有 n 个正整数的数组和一个正整数 s ，找出该数组中满足其和 ≥ s 的长度最小的 连续 子数组，并返回其长度。如果不存在符合条件的子数组，返回 0。

示例：
```
输入：s = 7, nums = [2,3,1,2,4,3]
输出：2
解释：子数组 [4,3] 是该条件下的长度最小的子数组。
```
来源：力扣（LeetCode）  
链接：https://leetcode-cn.com/problems/minimum-size-subarray-sum  
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 我的开始思路

- 暴力双循环

- 滑动窗口

## 代码

- 语言支持：C#
- 暴力双循环

```C#
        //时间复杂度O(N^2)
        //空间复杂度O(1)
        public int MinSubArrayLen(int s, int[] nums)
        {
            if (nums == null) return 0;

            int min = int.MaxValue;

            for (int i = 0; i < nums.Length; i++)
            {
                int sum = 0;

                for (int j = i; j < nums.Length; j++)
                {
                    sum += nums[j];
                    if (sum >= s)
                    {
                        if (j - i + 1 < min) min = j - i + 1;
                        break;
                    }
                }
            }

            return min == int.MaxValue ? 0 : min;
        }
```

- 语言支持：C#
- 滑动窗口

```C#
        //时间复杂度O(N)
        //空间复杂度O(1)
        public int MinSubArrayLen1(int s, int[] nums)
        {
            if (nums == null || nums.Length == 0) return 0;

            int left = 0;
            int right = 0;
            int sum = nums[0];
            int min = int.MaxValue;

            while (left < nums.Length)
            {
                if (sum >= s)
                {
                    min = right - left + 1 < min ? right - left + 1 : min;
                    sum -= nums[left];
                    left++;
                }
                else
                {
                    if (right < nums.Length - 1)
                    {
                        right++;
                        sum += nums[right];
                    }
                    else
                    {
                        break;
                    }
                }
            }

            return min == int.MaxValue ? 0 : min;
        }
```
