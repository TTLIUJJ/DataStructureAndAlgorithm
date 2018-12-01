# 判断二叉树是否为平衡二叉树（LeetCode 110）

利用二叉树后序遍历的规律：先访问节点的左子树和节点的右子树之后，当前节点可以获取左子树和右子树的高度，那么判断当前节点的子树是否平衡，依次递归上去。

```java
class Solution {
    private boolean mark = true;
    public boolean isBalanced(TreeNode root) {
        if (root == null) {
            return true;
        }
        getHeight(root, 0);
        return mark;
    }
    
    private int getHeight(TreeNode node, int h) {
        if (node == null) {
            return h;
        }
        int lh = getHeight(node.left,  h + 1);
        int rh = getHeight(node.right, h + 1);    
        if (Math.abs(lh - rh) > 1) {
            mark = false;
        }
        
        return Math.max(lh, rh);
    }
}
```