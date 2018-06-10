# Binary Search Tree

##  Validate Binary Search Tree

[https://www.lintcode.com/problem/validate-binary-search-tree/](https://www.lintcode.com/problem/validate-binary-search-tree/)

Version 1: Traverse

相当于对BST进行一个中序遍历，这个过程中依次检验

（1）左子树是BST

（2）当前root节点大于左子树中最大（即中序遍历最后一个）节点（除非是遍历整个树中遇到的的第一个node则不需要检验）

（3）右子树是BST（过程中同时会检验右子树中最小（即中序遍历第一个）节点大于当前root）

```java
public class Solution {
    private int lastVal = Integer.MIN_VALUE;
    private boolean firstNode = true;
    public boolean isValidBST(TreeNode root) {
        if(root==null) return true;
        if(!isValidBST(root.left)) return false;
        if(!firstNode && root.val<=lastVal) return false;
        lastVal=root.val;
        firstNode=false;
        if(!isValidBST(root.right)) return false;
        return true;
    }
}
```

Version 2: Divide & Conquer

```java
public class Solution {
    public boolean isValidBST(TreeNode root) {
      return helper(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }
    //min-max is the range that root should be in
    private boolean helper(TreeNode root, long min, long max){
      if(root==null) return true;
      if(root.val<=min || root.val>=max) return false;
      //check left/right root with min/max updated with root.val
      return helper(root.left,min,root.val) && helper(root.right,root.val,max);
    }
}
```

## Inorder successor of BST

[https://www.lintcode.com/problem/inorder-successor-in-binary-search-tree/](https://www.lintcode.com/problem/inorder-successor-in-binary-search-tree/)

思路

观察BST特点可以总结出，一个BST node p的inorder successor位置有如下特征：

1. 不可能出现在p的左子树中
2. 如果p有右子树，则其inorder successor在p的右子树中，是对右子树进行前序遍历第一个节点
3. 如果p没有右子树，则其inorder successor是p的某个ancestor，是从p往上走遇到的第一个比p大的ancestor，也就是从root往p走遇到的最后一个比p大的ancestor
4. 如果p没有右子树也没有比p大的ancestor，则p是整个BST最大的node，不存在inorder successor

```java
public class Solution {
    /*
     * @param root: The root of the BST.
     * @param p: You need find the successor node of p.
     * @return: Successor of p.
     */
    public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
    
        if(root==null || p==null) return null;
      
  // if p has right sub-tree, then return first inorder node of right sub-tree
        if(p.right!=null){
          TreeNode curr=p.right;
          while(curr.left!=null) curr=curr.left;
          return curr;
        }
      
  //if p doesn't have right sub-tree, then find last/closest ancestor that is larger than p
        TreeNode successor=null;
        while(root!=p && root!=null){
          if(root.val>p.val) {
            successor=root;
            root=root.left;
          }
          else root=root.right;
        }
        return successor;
    }
    
}
```

## Insert Node in a Binary Search Tree

[https://www.lintcode.com/en/problem/insert-node-in-a-binary-search-tree/](https://www.lintcode.com/en/problem/insert-node-in-a-binary-search-tree/)

Given a binary search tree and a new tree node, insert the node into the tree. You should keep the tree still be a valid binary search tree.

Recursion:

```java
public class Solution {
    /*
     * @param root: The root of the binary search tree.
     * @param node: insert this node into the binary search tree
     * @return: The root of the new binary search tree.
     */
    public TreeNode insertNode(TreeNode root, TreeNode node) {
        // write your code here
        if(root==null){
            return node;
        }
        if(node.val<root.val) root.left=insertNode(root.left,node);
        else root.right=insertNode(root.right,node);
        return root;
    }
}
```

Non-Recursion:

```java
public class Solution {
    /*
     * @param root: The root of the binary search tree.
     * @param node: insert this node into the binary search tree
     * @return: The root of the new binary search tree.
     */
    public TreeNode insertNode(TreeNode root, TreeNode node) {
        // write your code here
        if(root==null) return node;
        TreeNode last=root;
        while(last.left!=null || last.right!=null){
            if(node.val<last.val) {
                if(last.left!=null) last=last.left;
                else break;
            }
            else {
                if(last.right!=null) last=last.right;
                else break;
            }
        }
        if(node.val<last.val) last.left=node;
        else last.right=node;
        return root;
    }
}
```

## Remove Node in Binary Search Tree

[https://www.lintcode.com/problem/remove-node-in-binary-search-tree/](https://www.lintcode.com/problem/remove-node-in-binary-search-tree/)

Given a root of Binary Search Tree with unique value for each node. Remove the node with given value. If there is no such a node with given value in the binary search tree, do nothing. You should keep the tree still a binary search tree after removal.

Hibbard deletion algorithm:

[ http://www.mathcs.emory.edu/~cheung/Courses/171/Syllabus/9-BinTree/BST-delete.html](%20http://www.mathcs.emory.edu/~cheung/Courses/171/Syllabus/9-BinTree/BST-delete.html
)

![](../.gitbook/assets/image%20%281%29.png)

```java
public class Solution {
    /*
     * @param root: The root of the binary search tree.
     * @param value: Remove the node with given value.
     * @return: The root of the binary search tree after removal.
     */
    public TreeNode removeNode(TreeNode root, int value) {
        // write your code here
        if(root==null) return null;
        
        //node to delete is in left subtree
        if(value<root.val) root.left=removeNode(root.left, value);
        
        //node to delete is in right subtree
        else if(value>root.val) root.right=removeNode(root.right, value);
        
        //found node to delete
        else{
            //if one subtree is null, simply replace
            if(root.left==null) return root.right;
            if(root.right==null) return root.left;
            
            //if has both left&right subtree,
            //replace with successor (min in right subtree)
            TreeNode minInRight=root.right;
            while(minInRight.left!=null) minInRight=minInRight.left; //find successor
            minInRight.right=removeNode(root.right, minInRight.val);
            minInRight.left=root.left;
            root=minInRight;
        }
        return root;
    }
}
```

