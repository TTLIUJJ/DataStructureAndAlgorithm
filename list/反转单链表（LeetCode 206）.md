# 反转单链表（LeetCode 206）


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
    public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        
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
}
```

**递归版本：**

```java
/**
 * 递归过程
 *	1->2->3->4->5->6->7->null
 *	2->3->4->5->6->7->null
 *	3->4->5->6->7->null
 *	4->5->6->7->null
 *	5->6->7->null
 *	6->7->null
 *	7->null
 */
public class ReverseList {
	/**
	 * 当head==7时, head.next==null, 结束递归调用
	 * 此时唯一的返回值cur=7, 执行reverse(6), 
	 *     revere(6)执行6.next.next=6
	 *     即 将6->7改为7->6
	 * 结束reverse(6)的调用，
	 * 		接下来执行reverse(5), 5.next.next=5;
	 * 	 	即 将5->6改为6->5
	 * 	  ...
	 * 	最后返回cur=7
	 */
    public static ListNode reverse(ListNode head) {
        System.out.println(ListUtil.listToString(head));
        if (head == null || head.next == null) {
            return head;
        }

        ListNode cur = reverse(head.next);
        head.next.next = head;
        head.next = null;

        return cur;
    }
	
    public static void main(String []args) {
        ListNode head = ListUtil.createList4();
        head = reverse(head);

        System.out.println("\n\n" + ListUtil.listToString(head));

    }
}
```