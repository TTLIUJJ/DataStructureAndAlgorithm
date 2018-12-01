# 二叉树Zigzag遍历


```java
/**
 *                        1
 *          2                          3
 *                4               5           6
 *             7     8         9    10
 *                     11   12
 *                   13 14 15 16
 
 
 Z字形遍历的结果：
1 
3 2 
4 5 6 
10 9 8 7 
11 12 
16 15 14 13
 
 队列queue：用于暂存节点元素
 
 1          2 3
 2 3        4 5 6 (2 3)       队尾出 队头入
 4 5 6     (4 5 6) 7 8 9 10   队头出 队尾入  
 7 8 9 10   11 12 (7 8 9 10)  队尾出 队头入  
 */
 
 /**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> lists = new LinkedList<>();
        Deque<TreeNode> deque = new LinkedList<>();
        if (root == null) {
            return lists;
        }
        
        deque.add(root);
        boolean dequeFromHead = true;	// deque中节点入队的方向
        
        while (!deque.isEmpty()) {
            int size = deque.size();
            List<Integer> list = new LinkedList<>();
            while (size-- != 0) {
                if (dequeFromHead) {
                    TreeNode node = deque.pollFirst();
                    list.add(node.val);
                    if (node.left != null) {
                        deque.offerLast(node.left);
                    }
                    if (node.right != null) {
                        deque.offerLast(node.right);
                    }
                }
                else {
                    TreeNode node = deque.pollLast();
                    list.add(node.val);
                    if (node.right != null) {
                        deque.offerFirst(node.right);
                    }
                    if (node.left != null) {
                        deque.offerFirst(node.left);
                    }
                }
            }
            dequeFromHead = !dequeFromHead;
            lists.add(list);
        }
        return lists;
    }
}
 		
```