## 题目描述

```
给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有满足条件且不重复的三元组。

注意：答案中不可以包含重复的三元组。

示例：

给定数组 nums = [-1, 0, 1, 2, -1, -4]，

满足要求的三元组集合为：
[
  [-1, 0, 1],
  [-1, -1, 2]
]

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/longest-common-prefix
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

## 我的开始思路

暴力解法，三层循环，时间复杂度（N^3)

## 代码

- 语言支持：C#

```C#
        private IList<IList<int>> ThreeSum1(int[] nums)
        {
            IList<IList<int>> tmpList = new List<IList<int>>();
            int[] tmplist = new int[3];

            for (int i = 0; i < nums.Length - 2; i++)
            {
                for (int j = i + 1; j < nums.Length - 1; j++)
                {
                    int tmpInt = 0 - nums[i] - nums[j];

                    for (int n = j + 1; n < nums.Length; n++)
                    {
                        if (nums[n] == tmpInt)
                        {

                            tmplist[0] = nums[i];
                            tmplist[1] = nums[j];
                            tmplist[2] = nums[n];

                            if (IsContains(tmplist, tmpList) == false)
                            {
                                tmpList.Add(tmplist);
                                break;
                            }
                        }
                    }
                }
            }
            return tmpList;
        }

        private bool IsContains(int[] ts, IList<IList<int>> tsAll)
        {

            foreach (IList<int> item in tsAll)
            {
                if (ts[0] == item[0])
                {
                    if (ts[1] == item[1] && ts[2] == item[2]) return true;
                    if (ts[1] == item[2] && ts[2] == item[1]) return true;
                }

                if (ts[0] == item[1])
                {
                    if (ts[1] == item[0] && ts[2] == item[2]) return true;
                    if (ts[1] == item[2] && ts[2] == item[0]) return true;
                }

                if (ts[0] == item[2])
                {
                    if (ts[1] == item[1] && ts[2] == item[0]) return true;
                    if (ts[1] == item[0] && ts[2] == item[1]) return true;
                }
            }
            return false;
        }
```

## 学习他人的思路

排序后维护双指针，时间复杂度（N^2)；
再通过一些判断，来剪枝，从而继续减少运行次数。

## 代码

- 语言支持：C#

```C#
        IList<IList<int>> result = new List<IList<int>>();
            int left;
            int right;
            int sum;

            Array.Sort(nums);

            for (int i = 0; i < nums.Length - 2; i++)
            {
                if (nums[i] > 0) break;
                if (i > 0 && nums[i] == nums[i - 1]) continue; //去重

                left = i + 1;
                right = nums.Length - 1;

                if (nums[i] + nums[left] > 0) break;

                while (left < right)
                {
                    sum = nums[i] + nums[left] + nums[right];

                    if (sum == 0)
                    {
                        IList<int> list = new List<int>();
                        list.Add(nums[i]);
                        list.Add(nums[left]);
                        list.Add(nums[right]);

                        result.Add(list);

                        while (left < right && nums[left] == nums[left + 1]) left++; // 去重
                        while (left < right && nums[right] == nums[right - 1]) right--; // 去重
                        left++;
                        right--;

                    }
                    else if (sum > 0)
                    {
                        right--;
                    }
                    else
                    {
                        left++;
                    }
                }
            }

            return result;
        }
```

## 所学、所悟

- 有序数组

    - 重复的数字在一起，可以直接跳过，并且不会出现相同的list，list中的list是否相同并不好判断
    - 有序就有了规律，比如这道题目，当第一个数，或前两个数的和大于零后，就不需要在找了，可以减少很多次循环次数

- 当暴力方法需要N次循环的时候，要相当是否可以通过指针(如这道题的前后指针相遇运行)来减少时间复杂度
