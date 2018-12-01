# 统计完全二叉树的节点数（LeetCode 222）

时间复杂度O(h^2)

```java
/**
 * t1树：
 *	1          o
 *	2     o         o
 *	3   o   o     o   o
 *	4  o o o  o  o    
 *	  
 *	t2树：
 *	1          o
 *	2     o         o
 *	3   o   o     o   o
 *	4  o o o  
 *	
 *	找node右子树的最左节点
 * 		- 如果最左节点所在的层数level == t树的高度height
 * 	 	  那么node的左子树为满二叉树，
 * 	 	  node左子树+node总共的节点数：2^(height-level)-1+1
 * 	 	  即为2^(height-level)
 *		- 如果最左节点所在的层数level != t树的高度height 	 	    
 *	 	  那么node的右子树为满二叉树，
 *	 	  node右子树+node总共的节点数：2^(height-level-1)-1+1
 *	 	  即为2^(height-level-1)
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
    public int countNodes(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int height = mostLeafLevel(root, 1);
        return bs(root, 1, height);
    }
    
    private int bs(TreeNode root, int level, int height) {
        if (level == height) {
            return 1;
        }
        if (mostLeafLevel(root.right, level+1) == height) {
            return (1 << (height - level)) + bs(root.right, level+1, height);
        }
        else {
            return (1 << (height - level -1)) + bs(root.left, level+1, height);
        }
    }
    
    /** 计算叶节点所在的层数 */
    private int mostLeafLevel(TreeNode root, int level) {
        while (root != null) {
            ++level;
            root = root.left;
        }
        return level - 1;
    }
}
```