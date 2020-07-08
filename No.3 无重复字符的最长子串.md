## 题目描述

```
给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

示例 1:

输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
示例 2:

输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
示例 3:

输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/longest-substring-without-repeating-characters
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

## 我的开始思路

记录两个位置，一前一后，前位置向前遍历，每次检索该位置字符在两位置中是否有重复，无则继续向前，有则把后面的位置记录成前面重复的下一个，不断循环，维护长度。
其实就是滑动窗口，只是开始时没有滑动窗口的概念。

## 代码

- 语言支持：C#

```C#
        private int LengthOfLongestSubstring1(string s)
        {
            if (s == "") return 0;

            int tmpLong = 1;
            int tmpMaxLong = 0;

            int tmpTag = 0;

            char[] strCharArr = s.ToCharArray();

            if (strCharArr.Length == 1) return 1;

            for (int i = 0; i < strCharArr.Length - 1; i++)
            {
                for (int j = tmpTag; j <= i; j++)
                {
                    if (strCharArr[j] != strCharArr[i + 1])
                    {
                        tmpLong++;
                    }
                    else
                    {
                        tmpTag = j + 1;
                    }
                }

                if (tmpLong > tmpMaxLong)
                {
                    tmpMaxLong = tmpLong;
                }
                tmpLong = 1;
            }
            return tmpMaxLong;
        }
```

## 学习他人的思路

用数组来维护最后一次重复字符出现的位置，重复后把left更新，目前差值和前结果不断比较

## 代码

- 语言支持：C#

```C#
        private int lengthOfLongestSubstring(string s)
        {
            int[] last = new int[128];
            for (int i = 0; i < last.Length; ++i)
            {
                last[i] = -1;
            }
            int left = -1;
            int ans = 0;
            for (int i = 0; i < s.Length; ++i)
            {
                left = Math.Max(left, last[s[i]]);
                last[s[i]] = i;
                ans = Math.Max(ans, i - left);
            }
            return ans;

        }
```

## 学习到的知识和思路

- 感觉根本思想还是滑窗，不同的是用数组来维护，但没有悟道自己的东西


