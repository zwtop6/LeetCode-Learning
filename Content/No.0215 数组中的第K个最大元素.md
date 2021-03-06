## 题目描述

在未排序的数组中找到第 k 个最大的元素。请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。

示例 1:
```
输入: [3,2,1,5,6,4] 和 k = 2
输出: 5
```
示例 2:
```
输入: [3,2,3,1,2,4,5,5,6] 和 k = 4
输出: 4
```
说明:

你可以假设 k 总是有效的，且 1 ≤ k ≤ 数组的长度。

来源：力扣（LeetCode）  
链接：https://leetcode-cn.com/problems/kth-largest-element-in-an-array  
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 我的开始思路

调用库函数

本以为这个题只是让练习手动实现快速排序，结果是可以优化快速排序，还是太年轻了

## 代码

- 语言支持：C#

```C#
    public int FindKthLargest(int[] nums, int k) 
    {
        Array.Sort(nums);

        return nums[nums.Length-k];
    }
```

## 学习他人的思路

- 基于快速排序的选择排序

1. 因为只关心第k个大值，所以没必要全部排完，可以根据每次排好的位置来判断出接下来应该排哪一边，或者已经是结果了就不继续排序了

1. 运用随机数来帮助选择，快了很多，官方说时间复杂度期望为O(N)（来自《算法导论》，工作稳定后，一定要看看这本书）

1. 通过先把选中的数和最后一个数替换，最后在换回来，从而可以达到不需要额外空间

- 堆排序

1. 排一个大顶堆，然后删除最大值，在继续排，直到删除要求值

1. 直接把数组堪称堆来操作，学习学习

## 代码

- 语言支持：C#
- 手动快速排序

```C#
        public void QuickSorts(int[] nums, int left, int right)
        {
            int mid = Parition(nums, left, right);

            if (left < mid)
            {
                QuickSorts(nums, left, mid - 1);
            }

            if (right > mid)
            {
                QuickSorts(nums, mid + 1, right);
            }
        }

        private int Parition(int[] nums, int left, int right)
        {
            //经粗略实验，如果不搭配选择排序，反而慢了
            //int mid = new Random().Next(left, right);
            int mid = (left + right) / 2;

            int cur = left;

            Swap(nums, mid, right);

            for (int i = left; i < right; i++)
            {
                if (nums[i] < nums[right])
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
            int tmp = nums[i];

            nums[i] = nums[j];
            nums[j] = tmp;
        }
```

- 语言支持：C#
- 基于快速排序的选择排序

```C#
        private int QuickSorts(int[] nums, int left, int right,int ans)
        {
            int mid = Parition(nums, left, right);
            
            //对排好序的那个值进行判断，从而剪枝掉没必要的过程
            if (mid==ans) return nums[mid];
            else if (mid<ans)
            {
                return QuickSorts(nums, mid + 1, right,ans);
            }
            else
            {
                return QuickSorts(nums, left, mid - 1,ans);
            }

        }

        private int Parition(int[] nums, int left, int right)
        {
            //这里用随机就会很快了，Amazing！
            int mid = new Random().Next(left,right);
            //int mid = (left+right)/2;

            int cur = left;

            Swap(nums, mid, right);

            for (int i = left; i < right; i++)
            {
                if (nums[i] < nums[right])
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
        int tmp=nums[i];

        nums[i]=nums[j];
        nums[j]=tmp;
    }

```

- 语言支持：C#
- 堆排序

```C#
    public int FindKthLargest(int[] nums, int k) 
    {
        
        BuildMaxHeap(nums);

        for (int i=1;i<=k;i++)
        {
            DeleteMaxHeapPeak(nums,nums.Length-i);
        }

        return nums[nums.Length-k];
    }

    //创建堆顺序
    private void BuildMaxHeap(int[] nums)
    {
        //如果数组长度≤1，无须操作
        if (nums.Length<=1) return;

        int mid=nums.Length/2;

        for (int i=mid;i>=0;i--)
        {
            AdjustMaxHeap(nums,i,nums.Length-1);
        }
    }

    //删除大顶堆顶部(移到最后)
    private void DeleteMaxHeapPeak(int[] nums,int heapSize)
    {
        Swap(nums,0,heapSize);

        AdjustMaxHeap(nums,0,heapSize-1);
    }

    //调整部分堆
    private void AdjustMaxHeap(int[] nums, int cur, int heapSize)
    {
        int left=cur*2+1;
        int right=cur*2+2;
        int max=cur;

        if (right<=heapSize && nums[right]>nums[max])
        {
            max=right;
        }

        if (left<=heapSize && nums[left]>nums[max])
        {
            max=left;
        }

        if (max!=cur)
        {
            Swap(nums,cur,max);
            AdjustMaxHeap(nums,max,heapSize);
        }
    }

    private void Swap(int[] nums, int i, int j)
    {
        int tmp=nums[i];

        nums[i]=nums[j];
        nums[j]=tmp;
    }
```
