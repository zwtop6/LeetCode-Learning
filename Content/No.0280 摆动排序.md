## 题目描述

给你一个无序的数组 nums, 将该数字 原地 重排后使得 nums[0] <= nums[1] >= nums[2] <= nums[3]...。

示例:
```
输入: nums = [3,5,2,1,6,4]
输出: 一个可能的解答是 [3,5,1,6,2,4]
```
来源：力扣（LeetCode）  
链接：https://leetcode-cn.com/problems/wiggle-sort  
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 我的开始思路

遍历一遍比较即可，比较简单的一道题

## 代码

- 语言支持：C#

```C#
     public void WiggleSort(int[] nums) 
    {
        if (nums.Length<=1) return;

        bool isBiger=true;

        for (int i=1;i<nums.Length;i++)
        {
            if (isBiger)
            {
                if(nums[i]<nums[i-1])
                {
                    Swap(nums,i-1,i);
                }
            }
            else
            {
                if (nums[i]>nums[i-1])
                {
                    Swap(nums,i-1,i);
                }
            }

            isBiger=!isBiger;
        }
    }

    private void Swap(int[] nums,int i,int j)
    {
        int tmp=nums[i];

        nums[i]=nums[j];
        nums[j]=tmp;
    }
```
