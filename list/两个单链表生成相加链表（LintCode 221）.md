# 两个单链表生成相加链表（LintCode 221）

假定用一个链表表示两个数，其中每个节点仅包含一个数字。假设这两个数的数字顺序排列，请设计一种方法将两个数相加，并将其结果表现为链表的形式。

样例

```
给出 6->1->7 + 2->9->5。即，617 + 295。

返回 9->1->2。即，912 。
```


```java
public class AddLists {

    public static ListNode add(ListNode l1, ListNode l2) {
        // write your code here
        if (l1 == null) {
            return l2;
        }
        if (l2 == null) {
            return l1;
        }

        ListNode rList1 = reverse(l1);
        ListNode rList2 = reverse(l2);
        
        ListNode head1 = rList1;
        ListNode head2 = rList2;
        ListNode pre = null;
        ListNode cur = null;
        int rest = 0;
        int val1 = 0;
        int val2 = 0;
        int val  = 0;

        while (head1 != null || head2 != null) {
            val1 = head1 == null ? 0 : head1.val;
            val2 = head2 == null ? 0 : head2.val;
            val = rest + val1 + val2;

            pre = cur;
            cur = new ListNode(val % 10);
            rest = val / 10;
            cur.next = pre;

            head1 = head1 != null ? head1.next : null;
            head2 = head2 != null ? head2.next : null;
        }

        if (rest != 0) {
            pre = cur;
            cur = new ListNode(rest);
            cur.next = pre;
        }

        reverse(rList1);
        reverse(rList2);

        return cur;
    }

    private static ListNode reverse(ListNode head) {
        ListNode pre = null;
        ListNode cur = head;
        while (cur != null) {
            ListNode next = cur.next;
            cur.next = pre;
            pre = cur;
            cur = next;
        }

        return pre;
    }

    public static void main(String []args) {
        ListNode l1 = ListUtil.createList5();
        ListNode l2 = ListUtil.createList6();

        ListNode head = add(l1, l2);

        System.out.println(ListUtil.listToString(head));
    }
}

```