## 题目描述

运用你所掌握的数据结构，设计和实现一个  LRU (最近最少使用) 缓存机制。它应该支持以下操作： 获取数据 get 和 写入数据 put 。

获取数据 get(key) - 如果关键字 (key) 存在于缓存中，则获取关键字的值（总是正数），否则返回 -1。  
写入数据 put(key, value) - 如果关键字已经存在，则变更其数据值；如果关键字不存在，则插入该组「关键字/值」。  
当缓存容量达到上限时，它应该在写入新数据之前删除最久未使用的数据值，从而为新的数据值留出空间。

进阶:

你是否可以在 O(1) 时间复杂度内完成这两种操作？

示例:
```
LRUCache cache = new LRUCache( 2 /* 缓存容量 */ );

cache.put(1, 1);
cache.put(2, 2);
cache.get(1);       // 返回  1
cache.put(3, 3);    // 该操作会使得关键字 2 作废
cache.get(2);       // 返回 -1 (未找到)
cache.put(4, 4);    // 该操作会使得关键字 1 作废
cache.get(1);       // 返回 -1 (未找到)
cache.get(3);       // 返回  3
cache.get(4);       // 返回  4
```

来源：力扣（LeetCode）  
链接：https://leetcode-cn.com/problems/lru-cache  
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 我的开始思路

要求时间复杂度为O(1)，首先想到了Hashmap，直接把key和value放入其中

然后这道题还有另外两个要求，一个是容量上限，一个是最近最少使用

容量上限记录并判断一下即可，那么难点就在于最近最少使用

一开始想到了队列，用了就放进去，最开头的是很久没用的了

但是，当get时无法O(1)把中间的取数来，而且也不符合队列的使用

于是想到要方便取出和插入，那么就用list列表

问题也在于没办法通过key来直接O(1)取到，那么想O(1)，就得Hashmap

那么再维护一个Hashmap来记录key和list的item

感觉可以了，写好代码后提交发现不行，跟踪代码，发现当移动list时其余的位置全部发生了变化

那么就得再全部维护一遍Hashmap中的value，又变成了O(N)

于是不知道该怎么办了

学习了一下别人的思路，按照思路实现

## 代码

- 语言支持：C#

```C#
        //双链表
        public class ListTwoNode
        {
            public int key;
            public int value;
            public ListTwoNode pre;
            public ListTwoNode next;

            public ListTwoNode(int key, int value)
            {
                this.key = key;
                this.value = value;
            }

            public ListTwoNode()
            {
                this.key = 0;
                this.value = 0;
            }
        }

        public class LRUCache
        {
            int dictCapacity = 0;
            Dictionary<int, ListTwoNode> dict = new Dictionary<int, ListTwoNode>();

            ListTwoNode head;
            ListTwoNode end;

            private void RemoveKey(int key)
            {
                var tmp = dict[key];

                if (tmp == head)
                {
                    if (head.next != null)
                    {
                        head = head.next;
                        tmp.next.pre = null;
                        tmp.next = null;
                    }
                    else
                    {
                        head = null;    
                    }
                    
                }
                else if (tmp == end)
                {
                    end = end.pre;
                    tmp.pre.next = null;
                    tmp.pre = null;
                }
                else
                {
                    tmp.pre.next = tmp.next;
                    tmp.next.pre = tmp.pre;
                    tmp.next = null;
                    tmp.pre = null;
                }
            }

            private void SetHead(int key)
            {
                var tmp = dict[key];

                if (head != null)
                {
                    tmp.next = head;
                    head.pre = tmp;
                    head = tmp;
                }
                else
                {
                    head = tmp;
                    end = tmp;
                }
            }

            public LRUCache(int capacity)
            {
                dictCapacity = capacity;
            }

            public int Get(int key)
            {
                if (dict.ContainsKey(key))
                {
                    RemoveKey(key);
                    SetHead(key);

                    return dict[key].value;
                }
                else
                {
                    return -1;
                }
            }

            public void Put(int key, int value)
            {
                ListTwoNode tmp = new ListTwoNode(key, value);

                if (dict.ContainsKey(key))
                {
                    RemoveKey(key);
                    dict[key] = tmp;
                    SetHead(key);

                }
                else
                {
                    if (dict.Count >= dictCapacity)
                    {
                        dict.Remove(end.key);
                        end = end.pre;
                        end.next = null;
                    }

                    dict.Add(key, tmp);
                    SetHead(key);
                }
            }
        }
```

## 学习他人的思路

再仔细比较自己的代码和官方的，发现自己的很不优雅

不优雅的原因主要是因为其中条件和边界的判断，造成代码可读性变差

利用fakeHade和fakeTail的思路来再次优化自己的代码

## 代码

- 语言支持：C#

```
        public class LRUCache1
        {
            int MaxCapacity = 0;
            Dictionary<int, ListTwoNode> dict = new Dictionary<int, ListTwoNode>();

            ListTwoNode fakeHead = new ListTwoNode();
            ListTwoNode fakeTail = new ListTwoNode();

            public LRUCache1(int capacity)
            {
                MaxCapacity = capacity;
                fakeHead.next = fakeTail;
                fakeTail.pre = fakeHead;
            }

            public int Get(int key)
            {
                if (dict.ContainsKey(key))
                {
                    RemoveNode(dict[key]);
                    AddToTail(dict[key]);
                    return dict[key].value;
                }

                return -1;
            }

            public void Put(int key, int value)
            {
                if (dict.ContainsKey(key))
                {
                    RemoveNode(dict[key]);
                    dict[key] = new ListTwoNode(key, value);
                    AddToTail(dict[key]);
                }
                else
                {
                    if (dict.Count == MaxCapacity)
                    {
                        dict.Remove(fakeHead.next.key);
                        RemoveNode(fakeHead.next);
                    }

                    dict.Add(key, new ListTwoNode(key, value));
                    AddToTail(dict[key]);
                }
            }

            private void AddToTail(ListTwoNode node)
            {
                node.pre = fakeTail.pre;
                node.next = fakeTail;
                node.pre.next = node;
                node.next.pre = node;
            }

            private void RemoveNode(ListTwoNode node)
            {
                node.pre.next = node.next;
                node.next.pre = node.pre;
                node.next = null;
                node.pre = null;
            }
        }
```       

## 所学、所悟

这样就优雅很多了

主要学习到两点：

1. 当操作链表时，巧用fakeHead来除去边界条件的判断

1. 用Hashmap和双向链表来达到最近最少使用的记录
