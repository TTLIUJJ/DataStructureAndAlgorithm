# 二叉树的序列化和反序列化（LeetCode 297）

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        if (root == null) {
            return "#!";
        }
        String res = root.val + "!";
        res += serialize(root.left);
        res += serialize(root.right);
        
        return res;
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        String []values = data.split("!");
        Queue<String> queue = new LinkedList<>();
        
        for (int i = 0; i < values.length; ++i) {
            queue.offer(values[i]);
        }
        return preOrderDeserialize(queue);
    }
    
    public TreeNode preOrderDeserialize(Queue<String> queue) {
        String s = queue.poll();
        if (s.equals("#")) {
            return null;
        }
        TreeNode head = new TreeNode(Integer.valueOf(s));
        head.left  = preOrderDeserialize(queue);
        head.right = preOrderDeserialize(queue);
        
        return head;
    }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.deserialize(codec.serialize(root));
```