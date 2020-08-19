## 题目描述

给定一个链表，删除链表的倒数第 n 个节点，并且返回链表的头结点。

示例：
```
给定一个链表: 1->2->3->4->5, 和 n = 2.

当删除了倒数第二个节点后，链表变为 1->2->3->5.
```
说明：

给定的 n 保证是有效的。

进阶：

你能尝试使用一趟扫描实现吗

来源：力扣（LeetCode）  
链接：https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list  
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 我的开始思路

利用队列，遍历列表

当队列数据个数小于n+1时，进栈；当队列数据不小于n+1时，出栈再进栈

最后，队列中的第一个数为要删掉数的前一个

并利用fakeHead来简化特殊处理，最后返回fakeHead.Next即可

## 代码

- 语言支持：C#

```C#
        public ListNode RemoveNthFromEnd(ListNode head, int n)
        {
            if (n == 0) return head;

            Queue<ListNode> queue = new Queue<ListNode>();

            ListNode fakeHead = new ListNode();
            fakeHead.next = head;
            var cur = fakeHead;

            while (cur != null)
            {
                if (queue.Count == n + 1) queue.Dequeue();

                queue.Enqueue(cur);

                cur = cur.next;
            }

            cur = queue.Dequeue();
            cur.next = queue.Dequeue().next;

            return fakeHead.next;
        }
```

## 学习他人的思路

用两个指针就行了，我却用了一个列表

用列表时空间复杂度为O(N),而两个指针则为O(1)

## 代码

- 语言支持：C#

```C#
        public ListNode RemoveNthFromEnd1(ListNode head, int n)
        {
            if (n == 0) return head;

            ListNode fakeHead = new ListNode();
            fakeHead.next = head;

            ListNode left = fakeHead;
            ListNode right = head;
            int count = 0;

            while (right != null)
            {
                count++;

                if (count <= n)
                {
                    right = right.next;
                }
                else
                {
                    right = right.next;
                    left = left.next;
                }
            }

            left.next = left.next.next;

            return fakeHead.next;
        }
```

## 所学、所悟

链表就想到fakeHead！

链表就想到fakeHead！

链表就想到fakeHead！

重要的事情说三遍

链表的操作一般都是用有限个指针来完成的

