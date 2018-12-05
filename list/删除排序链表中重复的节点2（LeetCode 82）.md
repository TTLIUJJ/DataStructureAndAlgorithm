# 删除排序链表中重复的节点2（LeetCode 82）


```
// 1->1->2->3->3-3->4->4->5->null
// 1->1->2->5->null
```
使用辅助头节点方法

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
    public ListNode deleteDuplicates(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode newHead = new ListNode(-1);
        newHead.next = head;
        ListNode pre = newHead;
        ListNode cur = head;
        
        while (pre.next != null) {
            cur = pre.next;
            while (cur.next != null && cur.next.val == cur.val) {
                cur = cur.next;
            }
            if (pre.next == cur) {
                pre = pre.next;
            }
            else {
                pre.next = cur.next;
            }
        }
        
        return newHead.next;
    }
}
```