# Binary Search Tree

##  Validate Binary Search Tree

[https://www.lintcode.com/problem/validate-binary-search-tree/](https://www.lintcode.com/problem/validate-binary-search-tree/)

Version 1: Traverse

相当于对BST进行一个中序遍历，这个过程中依次检验

（1）左子树是BST

（2）当前root节点大于左子树root（除非是遍历整个树中遇到的的第一个node则不需要检验）

（3）右子树是BST（过程中同时会检验右子树的root大于当前root）

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

