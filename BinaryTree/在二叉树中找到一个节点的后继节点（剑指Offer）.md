# 在二叉树中找到一个节点的后继节点（剑指Offer）




给定一个二叉树和其中的一个结点，请找出中序遍历顺序的下一个结点并且返回。注意，树中的结点不仅包含左右子结点，同时包含指向父结点的指针。

- 情况1：node有右子树，那么后继节点就是右子树上的最左节点
- 情况2：node没有右子树，
	- 如果node的父节点的左节点是node，那么父节点就是node的后继节点
	- 如果node的父节点的右节点是node，那么向上寻找node的父节点，记p = node.parent，如果p.left = node，返回p；否则迭代 node = p; p = p.parent。
	
- 情况3：如果在情况2中，cur的父节点为null，表示node没有后继节点

```java
/**
 *             6
 *      3               9
 *   1     4        8      10
 *    2      5    7		  
 *	中序遍历的结果：1 2 3 4 5 6 7 8 9 10  
 *	情况1：比如节点3，后继节点为4
 *	情况2：比如节点5，后继节点为6
 *	情况3：比如节点10，后继节点为null
 * 	  

public class TreeLinkNode {
    int val;
    TreeLinkNode left = null;
    TreeLinkNode right = null;
    TreeLinkNode next = null;

    TreeLinkNode(int val) {
        this.val = val;
    }
}
*/
public class Solution {
    public TreeLinkNode GetNext(TreeLinkNode pNode){
        if (pNode == null) {
            return null;
        }
        
        if (pNode.right != null) {
            pNode = pNode.right;
            while (pNode.left != null) {
                pNode = pNode.left;
            }
            return pNode;
        }
        
        TreeLinkNode p = pNode.next;
        while (p != null && p.left != pNode) {
            pNode = p;
            p = p.next;
        }
        
        return p;
    }
}
```