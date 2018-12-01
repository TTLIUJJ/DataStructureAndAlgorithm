# 通过有序数组生成平衡搜索二叉树（LeetCode 108）

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
    public TreeNode sortedArrayToBST(int[] nums) {
        if (nums == null || nums.length == 0) {
            return null;
        }
        
        return createBSTViaInorder(nums, 0, nums.length-1);
    }
    
    private TreeNode createBSTViaInorder(int []a, int lo, int hi) {
        if (lo > hi) {
            return null;
        }
        int mid = (lo + hi) / 2;
        TreeNode root = new TreeNode(a[mid]);
        root.left  = createBSTViaInorder(a, lo, mid-1);
        root.right = createBSTViaInorder(a, mid+1, hi);
        
        return root;
    }
}
```