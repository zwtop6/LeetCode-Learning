## 题目描述

序列化是将一个数据结构或者对象转换为连续的比特位的操作，进而可以将转换后的数据存储在一个文件或者内存中，同时也可以通过网络传输到另一个计算机环境，采取相反方式重构得到原数据。

请设计一个算法来实现二叉树的序列化与反序列化。这里不限定你的序列 / 反序列化算法执行逻辑，你只需要保证一个二叉树可以被序列化为一个字符串并且将这个字符串反序列化为原始的树结构。

示例: 
```
你可以将以下二叉树：

    1
   / \
  2   3
     / \
    4   5

序列化为 "[1,2,3,null,null,4,5]"
```
提示: 这与 LeetCode 目前使用的方式一致，详情请参阅 LeetCode 序列化二叉树的格式。你并非必须采取这种方式，你也可以采用其他的方法解决这个问题。

说明: 不要使用类的成员 / 全局 / 静态变量来存储状态，你的序列化和反序列化算法应该是无状态的。

来源：力扣（LeetCode）  
链接：https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree  
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 我的开始思路

- 法一

序列化：用先序遍历和中序遍历得到两个序列

反序列化：通过先序遍历和中序遍历唯一确定一个树

- 法二

用Leetcode上常用的数组表现形式

然而都在反序列化的时候失败了，写不出

## 学习他人的思路

- DFS/BFS

## 代码

- 语言支持：C#
- DFS

```C#
        // Encodes a tree to a single string.
        public string serialize(TreeNode root)
        {

            if (root == null) return "null,";

            StringBuilder strB = new StringBuilder();

            //先序遍历
            strB.Append(root.val + ",");
            strB.Append(serialize(root.left));
            strB.Append(serialize(root.right));

            return strB.ToString();
        }

        // Decodes your encoded data to tree.
        public TreeNode deserialize(string data)
        {

            //边界条件
            if (data == "") return null;

            //分data为list
            List<string> list = data.Split(",").ToList();
            list.RemoveAt(list.Count - 1);

            return BuildTree(list);
        }

        private TreeNode BuildTree(List<string> list)
        {
            if (list[0] == "null")
            {
                list.RemoveAt(0);
                return null;
            }

            TreeNode root = new TreeNode(Convert.ToInt32(list[0]));

            list.RemoveAt(0);

            root.left = BuildTree(list);
            root.right = BuildTree(list);

            return root;
        }
```

## 所学、所悟

这种开放式的题目不要给自己增加难度

如这道题，序列化的时候，如果去掉一些没必要的null，那么在反序列化的时候，旧会有很多情况要去处理

首先要保证可以做出来，是最重要的

## 扩展
