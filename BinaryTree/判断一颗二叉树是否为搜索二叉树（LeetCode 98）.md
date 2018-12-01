# 判断一颗二叉树是否为搜索二叉树（LeetCode 98）

中序遍历

**版本一：**


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
class Solution {
    public boolean isValidBST(TreeNode root) {
        if (root == null) {
            return true;
        }
        
        Stack<TreeNode> stack = new Stack<>();
        TreeNode cur = root;
        TreeNode pre = null;
        
        while (cur != null || !stack.isEmpty()) {
            while (cur != null) {
                stack.push(cur);
                cur = cur.left;
            }
            cur = stack.pop();
            if (pre != null && pre.val >= cur.val) {
                return false;
            }
            pre = cur;
            cur = cur.right;
        }
        
        return true;
    }
}
```


**版本二：**

```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        if (root == null) {
            return true;
        }
        TreeNode mostRight = null;
        TreeNode cur = root;
        TreeNode pre = null;
        
        while (cur != null) {
            mostRight = cur.left;
            if (mostRight != null) {
                while (mostRight.right != null && mostRight.right != cur) {
                    mostRight = mostRight.right;
                }
                if (mostRight.right == null) {
                    mostRight.right = cur;
                    cur = cur.left;
                    continue;
                }
                else {
                    mostRight.right = cur;
                }
            }
            if (pre != null && pre.val >= cur.val) {
                return false;
            }
            pre = cur;
            cur = cur.right;
            
        }
        return true;
    }
}
```