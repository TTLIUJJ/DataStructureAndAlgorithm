# 一种怪异的节点删除方法（LeetCode 237）

给定链表中的节点node，但不给整个链表的头节点。删除链表中给定的node节点。

注意：最后一个节点删除不了


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
    public void deleteNode(ListNode node) {
        if (node == null) {
            return;
        }
        if (node.next == null) {
            throw new RuntimeException("can not remove last node.");
        }
        node.val  = node.next.val;
        node.next = node.next.next;
    }
}
```