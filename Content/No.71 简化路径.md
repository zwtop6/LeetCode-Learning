## 题目描述

```
以 Unix 风格给出一个文件的绝对路径，你需要简化它。或者换句话说，将其转换为规范路径。

在 Unix 风格的文件系统中，一个点（.）表示当前目录本身；此外，两个点 （..） 表示将目录切换到上一级（指向父目录）；两者都可以是复杂相对路径的组成部分。更多信息请参阅：Linux / Unix中的绝对路径 vs 相对路径

请注意，返回的规范路径必须始终以斜杠 / 开头，并且两个目录名之间必须只有一个斜杠 /。最后一个目录名（如果存在）不能以 / 结尾。此外，规范路径必须是表示绝对路径的最短字符串。

 

示例 1：

输入："/home/"
输出："/home"
解释：注意，最后一个目录名后面没有斜杠。

示例 2：

输入："/../"
输出："/"
解释：从根目录向上一级是不可行的，因为根是你可以到达的最高级。

示例 3：

输入："/home//foo/"
输出："/home/foo"
解释：在规范路径中，多个连续斜杠需要用一个斜杠替换。

示例 4：

输入："/a/./b/../../c/"
输出："/c"

示例 5：

输入："/a/../../b/../c//.//"
输出："/c"

示例 6：

输入："/a//b////c/d//././/.."
输出："/a/b/c"
```

## 我的开始思路

用"/"来把路径分割成一个个字符串，根据字符串内容来做出动作：

“.”和“”什么都不做

“..”返回上一级，删除list中最后一个

有内容，则在list中增加

最后只需要用“/”和list结合即为结果


## 代码

- 语言支持：C#

```C#
        private string SimplifyPath1(string path)
        {
            StringBuilder stringBuilder = new StringBuilder();
            List<string> tmpList = new List<string>();
            string[] tmpStr2;

            tmpStr2 = path.Split("/");

            foreach (string item in tmpStr2)
            {
                switch (item)
                {
                    case "":
                        break;
                    case ".":
                        break;
                    case "..":
                        if (tmpList.Count > 0) tmpList.RemoveAt(tmpList.Count - 1);
                        break;
                    default:
                        tmpList.Add(item);
                        break;
                }
            }

            foreach (string item in tmpList)
            {
                stringBuilder.Append("/");
                stringBuilder.Append(item);
            }

            if (stringBuilder.ToString() == "") stringBuilder.Append("/");

            return stringBuilder.ToString();
        }
```

## 学习他人的思路

思路一样，区别在于栈和list，栈更符合这种情况

## 代码

- 语言支持：C#

```C#
        private int lengthOfLongestSubstring(string s)
        {
            Stack<string> s = new Stack<string>();

            string[] spiltArr = path.Split('/');
            for (int i = 0; i < spiltArr.Length; i++)
            {
                if (spiltArr[i] == "")
                {
                    continue;
                }

                if (spiltArr[i] == "..")
                {
                    if (s.Count > 0)
                    {
                        s.Pop();
                    }
                }
                else if (spiltArr[i] != ".")
                {
                    s.Push(spiltArr[i]);
                }
            }

            if (s.Count != 0)
            {
                StringBuilder sb = new StringBuilder();
                while (s.Count != 0)
                {
                    sb.Insert(0, s.Pop());
                    sb.Insert(0, "/");
                }
                return sb.ToString();
            }
            else
            {
                return "/";
            }
        }
```

## 所学、所悟

没有使用栈的思想，每次都是用list或数组，使得细节容易出错，逻辑较难全面，并且代码可读性很差

要不仅知道有这些数据结构，也要在写代码的时候用到这些结构


