# 在二叉树中找到两个节点的最近公共祖先（LeetCode 236）

使用后序遍历，可以先处理当前节点cur的左右子树：

- 情况1：cur等于null或者o1或者o2，返回cur
- 情况2：cur的左子树left和右子树都为null，说明后序遍历至cur节点时，左右子树都没有发现o1或者o2，返回null
- 情况3：cur的左右子树都不为null，说明后序遍历至cur节点时，o1向上递归和o2向上递归首次在cur节点时相遇，返回cur
- 情况4：cur的左右子树其中之一为null，另一个节点为o1或者o2，记为node，说明后序遍历至cur节点时，node已经最近公共祖先，也可能是最近公共祖先的祖先节点，不管如何，返回node。

```java
**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null || root == p || root == q) {
            return root;
        }
        TreeNode  left = lowestCommonAncestor( root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        
        if (left != null && right != null) {
            return root;
        }
        
        return left != null ? left : right; 
    }
}
```