# 删除排序链表中重复出现的节点（LeetCode 83）


```java
public class RemoveRepeatSortListNodes {
    public static ListNode remove(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode cur  = head;
        ListNode pre  = null;

        while (cur != null) {
            while (cur.next != null && cur.val == cur.next.val) {
                cur = cur.next;
            }
            if (pre != null) {
                pre.next = cur;
            }
            else {
                head = cur;
            }
            pre = cur;
            cur = cur.next;
        }

        return head;
    }

    // 1->2->2->3->3-3->4->4->5
    // 1->2->3->4->5
    public static void main(String []args) {
        ListNode head = ListUtil.createList7();
        head = remove(head);
        System.out.println(ListUtil.listToString(head));
    }
}
```