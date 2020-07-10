## 题目描述

```
给定一个只包含数字的字符串，复原它并返回所有可能的 IP 地址格式。

有效的 IP 地址正好由四个整数（每个整数位于 0 到 255 之间组成），整数之间用 '.' 分隔。

 
示例:

输入: "25525511135"
输出: ["255.255.11.135", "255.255.111.35"]

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/restore-ip-addresses
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```

## 我的开始思路

穷举，把字符串分成4份，检查每份是否符合条件

## 代码

- 语言支持：C#

```C#
        private IList<string> RestoreIpAddresses1(string s)
        {
            IList<string> tmpList = new List<string>();
            long[] tmpInt = new long[4];
            int tmpLong;

            if (s.Length < 4 || s.Length > 12) return tmpList;          //长度小于4或大于12一定不符合条件

            for (int i = 1; i < s.Length - 2; i++)              //第一份一定是从0开始，所以从第二份开始循环
            {
                for (int j = i + 1; j < s.Length - 1; j++)
                {
                    for (int m = j + 1; m < s.Length; m++)
                    {
                        tmpInt[0] = Convert.ToInt64(s.Substring(0, i));
                        tmpInt[1] = Convert.ToInt64(s.Substring(i, j - i));
                        tmpInt[2] = Convert.ToInt64(s.Substring(j, m - j));
                        tmpInt[3] = Convert.ToInt64(s.Substring(m, s.Length - m));
                        
                        //因为在上一步会使得0开头的0丢失，这里通过长度检验一下
                        tmpLong = tmpInt[3].ToString().Length + tmpInt[2].ToString().Length +
                            tmpInt[1].ToString().Length + tmpInt[0].ToString().Length;              

                        if (tmpLong == s.Length && tmpInt[0] < 256 && tmpInt[1] < 256 && tmpInt[2] < 256 && tmpInt[3] < 256)
                        {
                            tmpList.Add(tmpInt[0] + "." + tmpInt[1] + "." + tmpInt[2] + "." + tmpInt[3]);
                        }
                    }
                }
            }

            return tmpList;
        }
```

## 学习他人的思路

回溯+剪枝

## 所学、所悟

- 回溯

回溯算法实际上一个类似枚举的搜索尝试过程，主要是在搜索尝试过程中寻找问题的解，当发现已不满足求解条件时，就“回溯”返回，尝试别的路径。回溯法是一种选优搜索法，按选优条件向前搜索，以达到目标。但当探索到某一步时，发现原先选择并不优或达不到目标，就退回一步重新选择，这种走不通就退回再走的技术为回溯法，而满足回溯条件的某个状态的点称为“回溯点”。许多复杂的，规模较大的问题都可以使用回溯法，有“通用解题方法”的美称。

引用自 百度百科

- 剪枝

通过人为引入一些判断条件，从而减少穷举次数

