## 题目描述

给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

示例:

```
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807
```

来源：力扣（LeetCode）  
链接：https://leetcode-cn.com/problems/reverse-linked-list  
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 我的开始思路

- 分别转换两个链表为数字，然后相加得到和，在转换为链表

但是链表很长很长时，数的结构将不够用

- 直接遍历，边加边记录余数边生成结果链表

当最后余数为1时要在增加一个next，容易遗漏

## 代码

- 语言支持：C#
- 直接遍历

```C#
        public ListNode AddTwoNumbers1(ListNode l1, ListNode l2)
        {
            if (l1 == null && l2 != null) return l2;
            if (l1 != null && l2 == null) return l1;
            if (l1 == null && l2 == null) return null;

            int remain = 0;

            ListNode result = new ListNode(0);
            ListNode cur = result;

            result.val = (l1.val + l2.val) % 10;
            remain = (l1.val + l2.val) / 10;

            while (l1.next != null || l2.next != null)
            {
                if (l1.next != null && l2.next != null)
                {
                    l1 = l1.next;
                    l2 = l2.next;
                    ListNode tmp = new ListNode(((l1.val + l2.val + remain) % 10));
                    remain = (l1.val + l2.val + remain) / 10;
                    cur.next = tmp;
                    cur = cur.next;
                }
                else if (l1.next != null)
                {
                    l1 = l1.next;
                    ListNode tmp = new ListNode(((l1.val + remain) % 10));
                    remain = (l1.val + remain) / 10;
                    cur.next = tmp;
                    cur = cur.next;
                }
                else
                {
                    l2 = l2.next;
                    ListNode tmp = new ListNode(((l2.val + remain) % 10));
                    remain = (l2.val + remain) / 10;
                    cur.next = tmp;
                    cur = cur.next;
                }
            }

            if (remain != 0) cur.next = new ListNode(1);

            return result;
        }
```

- 语言支持：C#
- 取值相加在得链表，无法全部通过

```C#
        public ListNode AddTwoNumbers2(ListNode l1, ListNode l2)
        {
            if (l1 == null && l2 != null) return l2;
            if (l1 != null && l2 == null) return l1;
            if (l1 == null && l2 == null) return null;

            long one = l1.val;
            long two = l2.val;
            int num = 1;
            ListNode result;
            ListNode cur;

            while (l1.next != null)
            {
                l1 = l1.next;
                one += l1.val * Convert.ToInt64(Math.Pow(10, num));
                num++;
            }

            num = 1;

            while (l2.next != null)
            {
                l2 = l2.next;
                two += l2.val * Convert.ToInt64(Math.Pow(10, num));
                num++;
            }

            long ans = one + two;
            result = new ListNode(Convert.ToInt32(ans % 10));
            cur = result;
            ans /= 10;

            while (ans != 0)
            {
                ListNode tmp = new ListNode(Convert.ToInt32(ans % 10));
                cur.next = tmp;
                cur = cur.next;
                ans /= 10;
            }

            return result;
        }
```

## 学习他人的思路

没找见其他思路的解法

## 所学、所悟

提供了一种用链表来计算非常非常大的数相加的计算方法
