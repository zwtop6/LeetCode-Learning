## 题目描述

```
编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 ""。

示例 1:

输入: ["flower","flow","flight"]
输出: "fl"
示例 2:

输入: ["dog","racecar","car"]
输出: ""
解释: 输入不存在公共前缀。
说明:

所有输入只包含小写字母 a-z 。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/longest-common-prefix
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```

## 我的开始思路

写一个比较两个字符串的最长公共前缀函数，然后遍历字符串数组，不断把前一个结果和当前字符串传入比较两个字符串的函数。

## 代码

- 语言支持：C#

```C#
        private string LongestCommonPrefix1(string[] strs)
        {
            string tmpStr;

            if (strs.Length == 0) return "";            
            tmpStr = strs[0];                                      

            foreach (var item in strs)
            {
                tmpStr = CommomPrefixTwo(tmpStr, item);         //调用比较两个字符串的最长公共前缀函数
                if (tmpStr == "") return "";            //当结果为空时，不需要继续遍历
            }

            return tmpStr;
        }

        private string CommomPrefixTwo(string str1, string str2)
        {
            char[] tmpStr1;
            char[] tmpStr2;
            int tmpMin;

            string tmpStr = "";

            tmpStr1 = str1.ToCharArray();       //转化字符串为字符数组
            tmpStr2 = str2.ToCharArray();       //转化字符串为字符数组              

            tmpMin = Math.Min(tmpStr1.Length, tmpStr2.Length) - 1;      //得到两字符数组的最小长度-1

            for (int i = 0; i <= tmpMin; i++)
            {
                if (tmpStr1[i] == tmpStr2[i])
                {
                    tmpStr += tmpStr1[i];
                }
                else
                {
                    return tmpStr;
                }
            }
            return tmpStr;
        }
```

## 学习他人的思路

直接把所有的字符串比较，用try来避免数组超出索引范围

## 代码

```C#
        public string LongestCommonPrefix2(string[] strs)
        {
            int curpos = 0;
            int l = strs.Length;
            string res;
            if (l == 0) return "";
            StringBuilder ans = new StringBuilder();
            while (true)
            {
                try
                {
                    var curchar = strs[0][curpos];
                    for (int i = 0; i < l; i++)
                    {
                        if (strs[i][curpos] != curchar)
                        {
                            res = ans.ToString();
                            return res;
                        };
                    }
                    ans.Append(curchar);
                    curpos++;
                }
                catch { break; }
            }
            res = ans.ToString();
            return res;
        }
```

## 学习到的知识和思路

- StringBuilder操作字符串

    1. append：连接
    1. setCharAt(int i, char c)：将第 i 个代码单元设置为 c（可以理解为替换）
    1. insert(int offset, String str)：在指定位置之前插入字符(串)
    1. delete(int startIndex,int endIndex)：删除起始位置（含）到结尾位置（不含）之间的字符串

- 字符串数组可以用str[][]的形式来选择到每个char

- 用try{}catch{}来代替一些边界判断，简单、安全、可靠，Great！

## 改进自己的代码

```C#
        private string LongestCommonPrefix(string[] strs)
        {
            string tmpStr;

            if (strs.Length == 0) return "";
            tmpStr = strs[0];

            foreach (var item in strs)
            {
                tmpStr = CommomPrefixTwo(tmpStr, item);
                if (tmpStr == "") return "";
            }

            return tmpStr;
        }

        private string CommomPrefixTwo(string str1, string str2)
        {
            StringBuilder tmpStr = new StringBuilder();
            int i = 0;

            while (true)
            {
                try
                {
                    if (str1[i] == str2[i])
                    {
                        tmpStr.Append(str1[i]);
                        i++;
                    }
                    else
                    {
                        return tmpStr.ToString();
                    }
                }
                catch { break; }
            }

            return tmpStr.ToString();
        }
```
