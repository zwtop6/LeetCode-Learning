## 题目描述

实现一个 Trie (前缀树)，包含 insert, search, 和 startsWith 这三个操作。

示例:
```
Trie trie = new Trie();

trie.insert("apple");
trie.search("apple");   // 返回 true
trie.search("app");     // 返回 false
trie.startsWith("app"); // 返回 true
trie.insert("app");   
trie.search("app");     // 返回 true
```
说明:

你可以假设所有的输入都是由小写字母 a-z 构成的。  
保证所有输入均为非空字符串。

来源：力扣（LeetCode）  
链接：https://leetcode-cn.com/problems/implement-trie-prefix-tree  
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 我的开始思路

直接学习的，感觉难点在于数据结构的设计上，两个方面

1. 用字典来记录孩子结点

1. 用一个标记来记录当前字符段是否为一个单词

## 代码

- 语言支持：C#

```C#
 public class Trie {
    
    class trieTree
    {
        public char c;
        public Dictionary<char,trieTree> children=new Dictionary<char,trieTree>();
        public bool isWord;

        public trieTree(){
            isWord=false;
        }
        public trieTree(char c)
        {
            this.c=c;
            this.isWord=false;
        }
    }

    private trieTree trie; 

    /** Initialize your data structure here. */
    public Trie() {
        trie=new trieTree();
    }
    
    /** Inserts a word into the trie. */
    public void Insert(string word) {
        
        var tree=trie;

        for (int i=0;i<word.Length;i++)
        {
            if (!tree.children.ContainsKey(word[i]))
            {
                tree.children.Add(word[i],new trieTree(word[i]));
            }

            tree=tree.children[word[i]];

            if (i==word.Length-1)
            {
                tree.isWord=true;
            }
        }
    }
    
    /** Returns if the word is in the trie. */
    public bool Search(string word) {
        var tree=trie;

        for (int i=0;i<word.Length;i++)
        {
            if (!tree.children.ContainsKey(word[i]))
            {
                return false;
            }

            tree=tree.children[word[i]];

            if (i==word.Length-1 && tree.isWord==true)
            {
                return true;
            }
        }

        return false;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    public bool StartsWith(string prefix) {
        var tree=trie;

        for (int i=0;i<prefix.Length;i++)
        {
            if (!tree.children.ContainsKey(prefix[i]))
            {
                return false;
            }

            tree=tree.children[prefix[i]];
        }

        return true;
    }
}
```
