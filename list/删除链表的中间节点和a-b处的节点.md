# 删除链表的中间节点和a/b处的节点

问题一：删除单链表的中间节点

```java
/**
 * 1              不删除
 * 1->2           删除节点1
 * 1->2->3        删除节点2
 * 1->2->3->4     删除节点2
 * 1->2->3->4->5  删除节点3
 */
public class RemoveMiddleNode {
    /**
     *  双指针的运动轨迹：
     *      1 2 3 4 5
     *      p   q
     *        p     q
     *
     *  双指针的运动轨迹：
     *      1 2 3 4 5 6
     *      p   q
     *        p     q
     */
    public static ListNode removeNode(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        if (head.next.next == null) {
            return head.next;
        }

        ListNode p = head;
        ListNode q = head.next.next;
        while (q.next != null && q.next.next != null) {
            p = p.next;
            q = q.next.next;
        }
        p.next = p.next.next;

        return head;
    }

    public static void main(String []args) {
        ListNode list5 = ListUtil.createList1();
        list5 = removeNode(list5);
        System.out.println(ListUtil.listToString(list5));

        ListNode list6 = ListUtil.createList3();
        list6 = removeNode(list6);
        System.out.println(ListUtil.listToString(list6));
    }
}
```


问题二：删除单链表a/b处的节点

```java
/**
 * 如果链表长度为7, a=5, b=7
 * r = (7*5)/7 = 5.000 向上取整 删除第5个节点
 *
 * 如果链表长度为7, a=5, b=6
 * r = (7*5)/6 = 5.833 向上取整 删除第6个节点
 *
 * 如果链表长度为7, a=1, b=6
 * r = (7*1)/b = 1.166 向上取整 删除第2个节点
 */
public class RemoveNodeByRatio {
    public static ListNode removeNode(ListNode head, int a, int b) {
        if (head == null) {
            return null;
        }

        int len = getLength(head);
        int r = (int) Math.ceil((double) (a * len) / (double)b);

        if (r == 1) {
            head = head.next;
        }
        else if (r > 1) {
            // 1 2 3 4 删除3
            ListNode cur = head;
            while (--r != 1) {
                cur = cur.next;
            }
            cur.next = cur.next.next;
        }

        return head;
    }

    private static int getLength(ListNode head) {
        int cnt = 0;
        while (head != null) {
            ++cnt;
            head = head.next;
        }

        return cnt;
    }

    public static void main(String []args) {
        ListNode list7 = ListUtil.createList4();
        list7 = removeNode(list7, 5, 7);
        System.out.println(ListUtil.listToString(list7));

        list7 = ListUtil.createList4();
        list7 = removeNode(list7, 5, 6);
        System.out.println(ListUtil.listToString(list7));

        list7 = ListUtil.createList4();
        list7 = removeNode(list7, 1, 6);
        System.out.println(ListUtil.listToString(list7));
    }
}
```