# 删除链表中倒数第k个节点（LeetCode 19）



在单链表中删除倒数第k个节点

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    /**
     *           1 2 3 4 5
     * 倒数第5个             p=null, n=0;  ==> head=head.next;
     * 倒数第2个             p=null, n=-3; ==>  3.next = 5;
     */
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode cur = head;
        while (cur != null) {
            --n;
            cur = cur.next;
        }
        if (n == 0) {
            head = head.next;
        }
        else if (n < 0) {
            cur = head;
            while (++n != 0) {
                cur = cur.next;
            }
            cur.next = cur.next.next;
        }
        else {
            // 倒数第k个比链表还长 给定的参数错误
            return null;
        }
        
        return head;
    }
}
```