## 题目描述

给定一个整数数组和一个整数 k，你需要找到该数组中和为 k 的连续的子数组的个数。

示例 1 :
```
输入:nums = [1,1,1], k = 2
输出: 2 , [1,1] 与 [1,1] 为两种不同的情况。
```
说明 :

数组的长度为 [1, 20,000]。
数组中元素的范围是 [-1000, 1000] ，且整数 k 的范围是 [-1e7, 1e7]。

来源：力扣（LeetCode）  
链接：https://leetcode-cn.com/problems/search-insert-position  
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 我的开始思路

滑动窗口，窗口内数字和小于目标和，则右边界向右扩大窗口，如果小，则左边界向左减小窗口

但是

数组内容为整数，存在负数的情况，也就是说右边界向右移动，和不一定变大

那么这种思路就不可行，可行也相当于暴力解法

- 暴力求解

双循环，求和比较，时间复杂度O(N^2),空间复杂度O(1)

提交后发现时间排名十分靠后，而且分布图十分明显的分为了两部分，也就是说还有O(N*logN)或O(N)的解法

但局限于开始的思路，觉得无法知道扩大长度后和是增大还是减少，并没有其他思路

故赶紧学习他人思路，因为学会后肯定会进步

## 代码

- 语言支持：C#
- 暴力法

```C#
        public int SubarraySum1(int[] nums, int k)
        {
            int count = 0;

            for (int i = 0; i < nums.Length; ++i)
            {
                int sum = 0;

                for (int j = i; j < nums.Length; j++)
                {
                    sum += nums[j];
                    if (sum == k) count += 1;
                }
            }

            return count;
        }
```

## 学习他人的思路

- 通过前缀和求连续区间的和

## 代码

- 语言支持：C#

```
        public int SubarraySum2(int[] nums, int k)
        {
            Dictionary<int, int> diction = new Dictionary<int, int> { };
            diction.Add(0, 1);

            int count = 0;
            int sum = 0;

            for (int i = 0; i < nums.Length; i++)
            {
                sum += nums[i];

                if (diction.ContainsKey(sum - k))
                {
                    count += diction[sum - k];
                }

                if (diction.ContainsKey(sum))
                {
                    diction[sum]++;
                }
                else
                {
                    diction.Add(sum, 1);
                }
            }

            return count;
        }
```

## 所学、所悟

通过前缀和求连续区间的和！

通过前缀和求连续区间的和！

通过前缀和求连续区间的和！

把遍历到的数组,如nums[0....i....j]，可以看成两部分

第一部分是[0...i],已知该段的和

第二部分是[i+1,...j],要求的部分，等于sum[0...i...j]-sum[0...i]

因为我们要找和为k的部分，也就可以转化为找是否存在sum[0...i]等于sum[0...i...j]-k

那么只需要从头开始遍历，求sum[0...i]，并看hashmap中是否有sum-k

如果有则加上hashmap的value

然后把此次的sum值记录到hashmap中即可
