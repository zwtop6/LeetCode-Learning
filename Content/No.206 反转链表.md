## 题目描述

反转一个单链表。

示例:

```
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
```

进阶:
你可以迭代或递归地反转链表。你能否用两种方法解决这道题？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/reverse-linked-list
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 我的开始思路

遍历的同时更改链表

## 代码

- 语言支持：C#

```C#
        public ListNode ReverseList1(ListNode head)
        {
            //当head为null或者只有一个next时直接返回本事即可
            if (head == null || head.next == null) return head;

            ListNode listNode = head;
            ListNode preResult;
            ListNode result;

            result = head;
            result.next = null;
            preResult = result;

            while (listNode.next != null)
            {
                result = listNode;
                listNode = listNode.next;
                result.next = preResult;
                preResult = result;

            }

            listNode.next = preResult;

            return listNode;
        }
```

## 学习他人的思路

- 栈

- 递归

每次看到递归的解法都恍然大悟，但实则悟不了啊，还是会想不到，多积累吧

不过递归的时间复杂度很不理想

## 代码

- 语言支持：C#
- 递归

```
        public ListNode ReverseList2(ListNode head)
        {
            if (head == null || head.next == null) return head;

            //没有对newHead做操作，相当于一直记录了下来最后一个
            var newHead = ReverseList2(head.next);
            head.next.next = head;
            head.next = null;

            return newHead;
        }
```       

## 代码

- 语言支持：C#
- 栈

```
        public ListNode ReverseList3(ListNode head)
        {
            if (head == null || head.next == null) return head;

            Stack<ListNode> stackNode = new Stack<ListNode>();

            var cur = head;

            //进栈
            while (cur.next != null)
            {
                stackNode.Push(cur);
                cur = cur.next;
            }

            var result = cur;

            //出栈
            while (stackNode.Count != 0)
            {
                cur.next = stackNode.Pop();
                cur = cur.next;
            }

            cur.next = null;

            return result;
        }
```     

## 所学、所悟

看似简单的一题，又做了好久，加油吧！
