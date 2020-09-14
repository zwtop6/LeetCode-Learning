## 题目描述

给定一个非负整数数组 A， A 中一半整数是奇数，一半整数是偶数。

对数组进行排序，以便当 A[i] 为奇数时，i 也是奇数；当 A[i] 为偶数时， i 也是偶数。

你可以返回任何满足上述条件的数组作为答案。

示例：
```
输入：[4,2,5,7]
输出：[4,5,2,7]
解释：[4,7,2,5]，[2,5,4,7]，[2,7,4,5] 也会被接受。
```
提示：
```
2 <= A.length <= 20000
A.length % 2 == 0
0 <= A[i] <= 1000
```
来源：力扣（LeetCode）  
链接：https://leetcode-cn.com/problems/sort-array-by-parity-ii  
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 我的开始思路

两个指针，从左向右遍历，一个找不满足要求的数，当找到时，另一个向右遍历找可以满足要求的数，交换位置，继续寻找

## 代码

- 语言支持：C#

```C#
    public int[] SortArrayByParityII(int[] A) 
    {
        int index=0;

        for (int i=0;i<A.Length;i++)
        {
            if (i%2!=A[i]%2)
            {
                index=i+1;
                int tmp=i%2;

                while(index<A.Length)
                {
                    if ( A[index]%2==tmp)
                    {
                        Swap(A,i,index);
                        break;
                    }
                    else
                    {
                        index++;
                    }
                }
            }
        }

        return A;
    }

    private void Swap(int[] nums,int i,int j)
    {
        int tmp=nums[i];
        nums[i]=nums[j];
        nums[j]=tmp;
    }
```

## 学习他人的思路

可以从两方面优化：

1. 只需要排奇数或者偶数即可，另一个也就排好了，即两个指针可以跳着走

1. 求是否为偶数可以用&1来判断，毕竟位运算时计算机最快的运算

## 代码

- 语言支持：C#

```C#
    public int[] SortArrayByParityII(int[] A) 
    {
        int index=1;

        for (int i=0;i<A.Length;i+=2)
        {
            if ((A[i]&1)!=0)
            {
                while(index<A.Length)
                {
                    if ( (A[index] & 1 )==0)
                    {
                        Swap(A,i,index);
                        break;
                    }
                    else
                    {
                        index+=2;
                    }
                }
            }
        }

        return A;
    }

    private void Swap(int[] nums,int i,int j)
    {
        int tmp=nums[i];
        nums[i]=nums[j];
        nums[j]=tmp;
    } 
```

## 所学、所悟

1. 满足一定条件，分成两组或多组，其中最后一组是不需要去分的，其他的分完后最后一组一定是正确的一组

1. 位运算很强大，但要想成为自己的武器，要多学多思多总结多理解

## 扩展

- 位运算

1. 基础知识

    | 符号       | 名称    |  运算规则  |  备注  |
    | --------   | -----:   | :----: | :-----: |
    | &        | 与      |   都为1才为1    |     |
    | &#124;       | 或      |   有1则为1    |     |
    | ^        | 异或      |   不同则为1    |     |
    | ~        | 取反      |   1和0互变    |     |
    | <<        | 左移      |   左移若干位，高位丢弃，低位补0    |     |
    | >>        | 右移      |   右移若干位，低位丢弃，无符号数，高位补0，有符号数，不同编译器处理不一样    |     |

