## 题目描述

```
假设按照升序排序的数组在预先未知的某个点上进行了旋转。
( 例如，数组 [0,1,2,4,5,6,7] 可能变为 [4,5,6,7,0,1,2] )。
搜索一个给定的目标值，如果数组中存在这个目标值，则返回它的索引，否则返回 -1 。

你可以假设数组中不存在重复的元素。

你的算法时间复杂度必须是 O(log n) 级别。

示例 1:

输入: nums = [4,5,6,7,0,1,2], target = 0
输出: 4
示例 2:

输入: nums = [4,5,6,7,0,1,2], target = 3
输出: -1

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/max-area-of-island
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

## 我的开始思路

通过第一个数组和最后一个数组来判断该数组是否经过旋转，未旋转的第一个数小于最后一个数，旋转的大于  
如果未旋转，直接采用二分法  
如果旋转过，那么可以通过大小关系来判断应该是从前开始找还是从后开始找，直到找到目标数或者排序发生变化

未旋转这样处理有些时间复杂度应该是达不到要求的，但是通过了...  
向LeetCode提交了测试案例并说明了情况，等待回复...

## 代码

- 语言支持：C#

```C#
        private int Search1(int[] nums, int target)
        {
            if (nums.Length == 0 || nums == null) return -1;

            if (nums[0] > nums[nums.Length - 1])
            {
                //非正序
                if (target < nums[0] && target > nums[nums.Length - 1]) return -1;
                if (target >= nums[0])
                {
                    for (int i = 0; i < nums.Length - 1; i++)
                    {
                        if (target == nums[i]) return i;
                        if (nums[i] > nums[i + 1]) return -1;
                    }
                }
                if (target <= nums[nums.Length - 1])
                {
                    for (int i = nums.Length - 1; i > 0; i--)
                    {
                        if (target == nums[i]) return i;
                        if (nums[i] < nums[i - 1]) return -1;
                    }
                }
                return -1;
            }
            else
            {
                //二分查找
                if (target == nums[0]) return 0;
                if (target == nums[nums.Length - 1]) return nums.Length - 1;

                int left = 0;
                int right = nums.Length - 1;
                int mid;

                while (left < right - 1)
                {
                    mid = (right + left) / 2;

                    if (nums[mid] == target) return mid;
                    else if (nums[mid] < target) left = mid;
                    else right = mid;
                }
                return -1;
            }
        }
```

## 学习他人的思路

- 先用二分查找把数组分成两段,再分别用二分查找

- 边二分边判断

倾向于第一种，因为代码可读性更好

## 代码

- 语言支持：C#

```C#
private int Search2(int[] nums, int target)
        {
            if (nums == null || nums.Length == 0)
                return -1;
            int mid = -1, start = 0, end = nums.Length - 1;
            while (end - start > 1)
            {
                mid = (start + end) / 2;
                if (nums[mid] > nums[start])
                    start = mid;
                else
                    end = mid;
            }
            int index1 = binarySearch(nums, 0, end - 1, target);
            int index2 = binarySearch(nums, end, nums.Length - 1, target);
            if (index1 < 0 && index2 < 0) return -1;
            return index1 < 0 ? index2 : index1;
        }

        private int binarySearch(int[] nums, int start, int end, int target)
        {
            int mid;
            while (start <= end)
            {
                mid = (start + end) / 2;
                if (nums[mid] == target)
                    return mid;
                if (nums[mid] > target)
                    end = mid - 1;
                else
                    start = mid + 1;
            }
            return -1;
        }
```

## 所学、所悟

- 当一段数组由两部分有序数组构成可用二分查找来分成两段处理
