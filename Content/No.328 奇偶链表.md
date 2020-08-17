## 题目描述

给定一个单链表，把所有的奇数节点和偶数节点分别排在一起。请注意，这里的奇数节点和偶数节点指的是节点编号的奇偶性，而不是节点的值的奇偶性。

请尝试使用原地算法完成。你的算法的空间复杂度应为 O(1)，时间复杂度应为 O(nodes)，nodes 为节点总数。

示例 1:
```
输入: 1->2->3->4->5->NULL
输出: 1->3->5->2->4->NULL
```
示例 2:
```
输入: 2->1->3->5->6->4->7->NULL 
输出: 2->3->6->7->1->5->4->NULL
```
说明:

应当保持奇数节点和偶数节点的相对顺序。  
链表的第一个节点视为奇数节点，第二个节点视为偶数节点，以此类推。

来源：力扣（LeetCode）  
链接：https://leetcode-cn.com/problems/odd-even-linked-list  
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 我的开始思路

记录一个奇头和一个偶头，然后遍历，分别按顺序连接到奇头和偶头后

最后再按照总共是奇数个还是偶数个来做不同的处理

## 代码

- 语言支持：C#

```C#
        public ListNode OddEvenList(ListNode head) {
        
            if (head == null || head.next == null) return head;

            ListNode evenHead = head.next;
            ListNode tmpOdd = head;
            ListNode tmpEven = head.next;
            ListNode cur = head.next.next;

            int count = 3;

            while (cur != null)
            {
                if (count % 2 == 1)
                {
                    tmpOdd.next = cur;
                    tmpOdd = cur;
                }
                else
                {
                    tmpEven.next = cur;
                    tmpEven = cur;
                }

                cur = cur.next;
                count++;
            }

            if (count % 2 == 0)
            {
                tmpEven.next = null;
                tmpOdd.next = evenHead;
            }
            else
            {
                tmpOdd.next = evenHead;
            }

            return head;
    }
```

## 学习他人的思路

整体思路是一样的

但是有两个细节可以优化

1. 最后按照奇偶数来分别处理不需要，可以按照同一种来处理

1. 不需要记录个数，用个布尔值即可，这样不需要进行取余计算，直接布尔值比较和取反计算是很快的

## 调整后的代码

- 语言支持：C#

```C#
```
        public ListNode OddEvenList(ListNode head)
        {
            if (head == null || head.next == null) return head;

            ListNode evenHead = head.next;
            ListNode tmpOdd = head;
            ListNode tmpEven = head.next;
            ListNode cur = head.next.next;

            bool isOdd = true;

            while (cur != null)
            {
                if (isOdd)
                {
                    tmpOdd.next = cur;
                    tmpOdd = cur;
                }
                else
                {
                    tmpEven.next = cur;
                    tmpEven = cur;
                }

                cur = cur.next;
                isOdd = !isOdd;
            }

            tmpEven.next = null;
            tmpOdd.next = evenHead;

            return head;
        }
```
