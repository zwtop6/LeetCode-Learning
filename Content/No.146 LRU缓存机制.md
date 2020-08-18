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

## 代码

- 语言支持：C#

```

```       

## 所学、所悟

