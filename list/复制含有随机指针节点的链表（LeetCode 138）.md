# 复制含有随机指针节点的链表（LeetCode 138）

A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null.

Return a deep copy of the list.


```java
public class CopyRandomList {
    public static RandomListNode copyRandomList(RandomListNode head) {
        if (head == null) {
            return null;
        }

        RandomListNode cur = head;
        while (cur != null) {
            RandomListNode next = cur.next;
            RandomListNode copy = new RandomListNode(cur.label);
            cur.next = copy;
            copy.next = next;
            cur = next;
        }

        cur = head;
        while (cur != null ) {
            if (cur.random != null) {
                cur.next.random = cur.random.next;
            }
            cur = cur.next.next;
        }

        RandomListNode test = head;
        printRandomList(test);

        RandomListNode newHead = head.next;
        cur = newHead;
        RandomListNode headNext = null;
        RandomListNode curNext  = null;

        while (head != null) {
            headNext = head.next.next;
            if (headNext != null) {
                curNext = headNext.next;
                cur.next = curNext;
                cur = curNext;
            }
            head.next = headNext;
            head = headNext;
        }

        return newHead;
    }

    private static void printRandomList(RandomListNode node) {
        while (node != null) {
            System.out.print(node.label + "[" + (node.random == null ? "" :  node.random.label) + "]->");
            node = node.next;
        }
        System.out.println("null");
    }

    public static void main(String []args) {
        RandomListNode node1 = new RandomListNode(1);
        RandomListNode node2 = new RandomListNode(2);
        RandomListNode node3 = new RandomListNode(3);
        RandomListNode node4 = new RandomListNode(4);
        RandomListNode node5 = new RandomListNode(5);

        node1.next = node2;
        node2.next = node3;
        node3.next = node4;
        node4.next = node5;

        node2.random = node4;
        node5.random = node3;

        RandomListNode head = copyRandomList(node1);
        printRandomList(head);

    }
}
```