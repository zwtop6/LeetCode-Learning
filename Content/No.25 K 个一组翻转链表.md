## 题目描述

给你一个链表，每 k 个节点一组进行翻转，请你返回翻转后的链表。

k 是一个正整数，它的值小于或等于链表的长度。

如果节点总数不是 k 的整数倍，那么请将最后剩余的节点保持原有顺序。

示例：
```
给你这个链表：1->2->3->4->5  
当 k = 2 时，应当返回: 2->1->4->3->5  
当 k = 3 时，应当返回: 3->2->1->4->5
```
说明：

你的算法只能使用常数的额外空间。  
你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换。

来源：力扣（LeetCode）  
链接：https://leetcode-cn.com/problems/reverse-nodes-in-k-group  
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 我的开始思路

用一个最大长度为k的栈，不够k时进栈，到k时出栈并排序

## 代码

- 语言支持：C#

```C#
        public ListNode ReverseKGroup(ListNode head, int k)
        {
            Stack<ListNode> stack = new Stack<ListNode>();
            ListNode cur = head;
            ListNode last = null;

            int count = 0;

            while (cur != null)
            {
                count += 1;

                if (count < k)
                {
                    stack.Push(cur);
                    cur = cur.next;
                }
                else
                {
                    if (last == null) head = cur;
                    else last.next = cur;

                    var tmp = cur;
                    cur = cur.next;

                    while (stack.Count != 0)
                    {
                        tmp.next = stack.Pop();
                        tmp = tmp.next;
                    }

                    last = tmp;
                    last.next = null;
                    count = 0;
                }
            }

            //处理栈中剩余元素
            if (stack.Count != 0)
            {
                while (stack.Count > 1)
                {
                    stack.Pop();
                }

                last.next = stack.Pop();
            }

            return head;
        }
```

## 学习他人的思路

写一个函数用来反转指定位置中间的元素

## 代码

- 语言支持：C#

```
        public ListNode ReverseKGroup1(ListNode head, int k)
        {
            ListNode fakeHead = new ListNode(0);
            fakeHead.next = head;

            ListNode prev = fakeHead;
            ListNode cur = prev.next;

            int count = 0;

            while (cur != null)
            {
                count++;

                if (count % k != 0)
                {
                    cur = cur.next;
                    continue;
                }

                prev = ReverseGroup(prev, cur.next);
                cur = prev.next;
            }

            return fakeHead.next;
        }

        private ListNode ReverseGroup(ListNode prev, ListNode next)
        {
            ListNode last = prev.next;
            ListNode cur = last.next;

            while (cur != next)
            {
                ListNode tmp = cur.next;
                cur.next = prev.next;
                prev.next = cur;
                last.next = tmp;
                cur = tmp;
            }

            return last;
        }
```

## 所学、所悟

也想过用一个函数来写反转部分，但是考虑到开头，结尾要分别做处理，没想到好办法

有两个想法值得学习

1. 用fakeHead来使得不用特殊处理头节点部分

1. 反转函数传的是要反转部分的前一个和后一个listNode，这样就可以统一处理了

## 改进自己的代码

- 语言支持C#

```

```
