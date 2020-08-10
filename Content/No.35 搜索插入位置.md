## 题目描述

给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

你可以假设数组中无重复元素。

示例 1:
```
输入: [1,3,5,6], 5
输出: 2
```
示例 2:  
```
输入: [1,3,5,6], 2
输出: 1
```
示例 3:
```
输入: [1,3,5,6], 7
输出: 4
```
示例 4:
```
输入: [1,3,5,6], 0
输出: 0
```
来源：力扣（LeetCode）  
链接：https://leetcode-cn.com/problems/search-insert-position  
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 我的开始思路

二分法

## 代码

- 语言支持：C#

```C#
        public int SearchInsert(int[] nums, int target)
        {
            if (nums == null) return 0;

            int start = 0;
            int end = nums.Length - 1;

            while (start <= end)
            {
                int mid = (start + end) / 2;

                if (nums[mid] == target) return mid;
                else if (nums[mid] < target) start = mid + 1;
                else end = mid - 1;
            }

            return start;
        }
```

## 学习他人的思路

遍历

## 所学、所悟

实际中，可以根据不同的规模来应用不同的查找策略

没有最好的，只有最合适的
