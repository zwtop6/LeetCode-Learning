## 题目描述

给定一个排序数组，你需要在原地删除重复出现的元素，使得每个元素最多出现两次，返回移除后数组的新长度。

不要使用额外的数组空间，你必须在原地修改输入数组并在使用 O(1) 额外空间的条件下完成。

示例 1:
```
给定 nums = [1,1,1,2,2,3],

函数应返回新长度 length = 5, 并且原数组的前五个元素被修改为 1, 1, 2, 2, 3 。

你不需要考虑数组中超出新长度后面的元素。
```
示例 2:
```
给定 nums = [0,0,1,1,1,1,2,3,3],

函数应返回新长度 length = 7, 并且原数组的前五个元素被修改为 0, 0, 1, 1, 2, 3, 3 。

你不需要考虑数组中超出新长度后面的元素。
```
说明:

为什么返回数值是整数，但输出的答案是数组呢?

请注意，输入数组是以“引用”方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。

你可以想象内部操作如下:
```
// nums 是以“引用”方式传递的。也就是说，不对实参做任何拷贝
int len = removeDuplicates(nums);

// 在函数里修改输入数组对于调用者是可见的。
// 根据你的函数返回的长度, 它会打印出数组中该长度范围内的所有元素。
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```
来源：力扣（LeetCode）  
链接：https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array-ii  
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 我的开始思路

维护两个指针，一个指向要被赋值的位置，另一个不断向前遍历

但是，在写代码的时候发现要各种判断，才能确认被赋值指针的位置，故放弃

再想

只需要记录当前指针前总共重复的次数，把当前指针赋值到重复次数前的位置

最后返回总长度-总共重复次数

## 代码

- 语言支持：C#

```C#
        public int RemoveDuplicates1(int[] nums)
        {
            if (nums.Length <= 2) return nums.Length;

            int count = 0;
            int curNum = nums[0];
            int curCount = 1;

            for (int i = 1; i < nums.Length; i++)
            {
                nums[i - count] = nums[i];

                if (nums[i] == curNum)
                {
                    curCount++;
                }
                else
                {
                    curNum = nums[i];
                    curCount = 1;
                }

                if (curCount > 2)
                {
                    count++;
                }
            }

            return nums.Length - count;
        }
```

## 学习他人的思路

因为删除后的数组，最多重复数字只有两个，那么位置相差为2的一定不相等

那么只需要遍历的同时去还原即可

用两个指针，一个指向已经排好的前缀数组的下一个，另一个遍历

## 代码

- 语言支持：C#

```C#
        public int RemoveDuplicates2(int[] nums)
        {
            if (nums.Length <= 2) return nums.Length;

            int position = 2;

            for (int i = 2; i < nums.Length; i++)
            {
                if (nums[i] != nums[position - 2])
                {
                    nums[position++] = nums[i];
                }
            }
            
            return position;
        }
```

## 所学、所悟

这个思路就很像[No.42 接雨水](https://github.com/zwtop6/LeetCode-Learning/blob/master/Content/No.42%20%E6%8E%A5%E9%9B%A8%E6%B0%B4.md)这道题

去观察结果数组的规律，然后把当前数组向结果数组“凑”

因为一般结果会更加“整齐”，也就更好找到规律

[No.42 接雨水](https://github.com/zwtop6/LeetCode-Learning/blob/master/Content/No.42%20%E6%8E%A5%E9%9B%A8%E6%B0%B4.md)这道题就是把现有的数组凑成了“单峰”型

而这道题是凑成相差2不相等的数组
