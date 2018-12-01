# 调整二叉搜索树中两个错误的节点（LeetCode 99）

一颗二叉树原来是搜索二叉树，但是其中两个节点调换了位置，使得这颗二叉树你不再是搜索二叉树，请找到两个错误节点并调整其节点值。

```java
/**
 * 使用二叉树的中序遍历
 * 如果出现了两次降序：
 * 		第一个错误节点：第一次降序时较大的节点
 * 	 	第二个错误节点：第二次降序时较小的节点
 * 	如果出现了一次降序：
 *  	第一个错误节点：降序时较大的节点
 *   	第二个错误节点：降序时较小的节点
 *  
 *  以{1, 2, 3, 4, 5}为例：
 *  如果两次降序{1, 5, 3, 4, 2}：
 *   	那么第一个错误节点为5，第二个错误节点为2
 *  如果一次降序{1, 2, 4, 3, 5}：
 *  	那么第一个错误节点为4，第二个错误节点为3
 *  最后，交换两个错误节点的值即可
 * 	
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public void recoverTree(TreeNode root) {
        Stack<TreeNode> stack = new Stack<>();
        TreeNode []errs = new TreeNode[2];
        TreeNode cur = root;
        TreeNode pre = null;
        
        while (cur != null || !stack.isEmpty()) {
            while (cur != null) {
                stack.push(cur);
                cur = cur.left;
            }
            cur = stack.pop();
            if (pre != null && pre.val > cur.val) {
                errs[0] = errs[0] == null ? pre : errs[0];
                errs[1] = cur;
            }
            
            pre = cur;
            cur = cur.right;
        }
        
        int temp = errs[0].val;
        errs[0].val = errs[1].val;
        errs[1].val = temp;
    }
}

```


```java
class Solution {
    public void recoverTree(TreeNode root) {
        TreeNode []errs = new TreeNode[2];
        TreeNode pre = null;
        TreeNode cur = root;
        TreeNode mostRight = null;
        
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
                    mostRight.right = null;
                }
            }
            if (pre != null && pre.val > cur.val) {
                errs[0] = errs[0] == null ? pre : errs[0];
                errs[1] = cur;
            }
            pre = cur;
            cur = cur.right;
        }
        
        int temp = errs[0].val;
        errs[0].val = errs[1].val;
        errs[1].val = temp;
    }
}
```