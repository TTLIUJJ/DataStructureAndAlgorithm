# 将单链表的每K个节点之间逆序（LeetCode 25）


```java
public class ReverseKNodes {
    // 1 2 3    4 5 6    7
    // 3 2 1    6 5 4    7

    public static ListNode reverseKnodes(ListNode head, int k) {
        ListNode cur    = head;
        ListNode before = null; // 被逆转链表的前一个节点
        ListNode next   = null; // 被逆转链表的尾后节点, 同时也记录cur的下一个节点
        ListNode beg    = head; // 链表逆转之前的节点
        int cnt = 1;

        while (cur != null) {
            next = cur.next;
            if (cnt == k) {
                if (before != null) {
                    before.next = reverse(beg, cur.next);
                }
                else {
                    head = reverse(head, cur.next);
                }
                before = beg;
                beg = next;
                cnt = 0;
            }
            ++cnt;
            cur = next;
        }
        if (before != null) {
            before.next = beg;
        }

        return head;
    }

    // 返回逆转链表的新头节点
    private static ListNode reverse(ListNode head, ListNode tail) {
        ListNode pre  = null;
        ListNode cur  = head;
        ListNode next = null;

        while (cur != tail) {
            next = cur.next;
            cur.next = pre;
            pre = cur;
            cur = next;
        }

        ListNode test = pre;
        System.out.println("test: " + ListUtil.listToString(test));

        return pre;
    }

    public static void main(String []args) {
        ListNode head = ListUtil.createList4();
        head = reverseKnodes(head, 2);
        System.out.println(ListUtil.listToString(head));
    }
}
```