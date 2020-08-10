## 题目描述

斐波那契数，通常用 F(n) 表示，形成的序列称为斐波那契数列。该数列由 0 和 1 开始，后面的每一项数字都是前面两项数字的和。也就是：

F(0) = 0,   F(1) = 1
F(N) = F(N - 1) + F(N - 2), 其中 N > 1.
给定 N，计算 F(N)。

示例 1：
```
输入：2
输出：1
解释：F(2) = F(1) + F(0) = 1 + 0 = 1.
```
示例 2：
```
输入：3
输出：2
解释：F(3) = F(2) + F(1) = 1 + 1 = 2.
```
示例 3：
```
输入：4
输出：3
解释：F(4) = F(3) + F(2) = 2 + 1 = 3.
```

提示：

0 ≤ N ≤ 30

来源：力扣（LeetCode）  
链接：https://leetcode-cn.com/problems/fibonacci-number  
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 我的开始思路

递归

## 代码

- 语言支持：C#

```C#
        public int Fib(int N)
        {
            if (N <= 1) return N;

            return Fib(N - 1) + Fib(N - 2);
        }
```

## 学习他人的思路

直接加过去

## 代码

- 语言支持：C#

```C#
        public int Fib1(int N)
        {
            int result = 0;
            int pre = 1;
            int prepre = 2;

            if (N <= 1) return N;

            for (int i = 2; i <= N; i++)
            {
                result = pre + prepre;
                prepre = pre;
                pre = result;
            }

            return result;
        }
```

## 所学、所悟

没有分析时间复杂度，而且定式思维，没有多想

这里的递归：时间复杂度O(2^N)，空间复杂度O(N)

而直接加过去：时间复杂度O(N)，空间复杂度O(1)
