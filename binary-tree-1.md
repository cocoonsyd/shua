# Binary Tree

#### 碰到二叉树的问题，就想想整棵树在该问题上的结果和左右儿子在该问题上的结果之间的联系是什么



## Binary Tree Paths

[https://www.lintcode.com/problem/binary-tree-paths/](https://www.lintcode.com/problem/binary-tree-paths/)

 Given a binary tree, return all root-to-leaf paths.

Version 1, Divide & Conquer

```java
public class Solution {
    /**
     * @param root: the root of the binary tree
     * @return: all root-to-leaf paths
     */
    public List<String> binaryTreePaths(TreeNode root) {
        // write your code here
        List<String> result = new ArrayList<String>();
        if(root==null) return result;
        List<String> result_left = binaryTreePaths(root.left);
        List<String> result_right = binaryTreePaths(root.right);
        for (String path : result_left){
          result.add(root.val+"->"+path);
        }
        for (String path : result_right){
          result.add(root.val+"->"+path);
        }
        if(result.size()==0){
          result.add(Integer.toString(root.val));
        }
        return result;
    }
}
```

Version 2, Traverse

```java
public class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> result = new ArrayList<String>();
        if(root==null) return result;
        helper(result, root, Integer.toString(root.val));
        return result;
    }
    private void helper(List<String> result, TreeNode root, String path_so_far){
      if(root==null) return;
      if(root.left==null && root.right==null){
        //reach leaf node, add path to result
        result.add(path_so_far);
        return;
      }
      if(root.left!=null){
        helper(result,root.left,path_so_far+"->"+root.left.val);
      }
      if(root.right!=null){
        helper(result,root.right,path_so_far+"->"+root.right.val);
      }
    }
}
```

## Lowest Common Ancestor

[https://www.lintcode.com/problem/lowest-common-ancestor/](https://www.lintcode.com/problem/lowest-common-ancestor/)

```java
public class Solution {
    /*
     * @param root: The root of the binary search tree.
     * @param A: A TreeNode in a Binary.
     * @param B: A TreeNode in a Binary.
     * @return: Return the least common ancestor(LCA) of the two nodes.
     */
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode A, TreeNode B) {
        // write your code here
        if(root==null || root==A || root==B) return root;
        
        TreeNode left = lowestCommonAncestor(root.left, A, B);
        TreeNode right = lowestCommonAncestor(root.right, A, B);
        
        // A and B exist in left/right subtree respectively
        if(left!=null && right!=null) return root; //got LCA
        
        // A or B exist in left subtree
        if(left!=null) return left;
        
        // A or B exist in right subtree
        if(right!=null) return right;
        
        return null;
    }
}
```

##  Flatten Binary Tree to Linked List

[https://www.lintcode.com/problem/flatten-binary-tree-to-linked-list/](https://www.lintcode.com/problem/flatten-binary-tree-to-linked-list/)

Version 1, traversal: 相当于是对二叉树进行一次前序遍历

```java
public class Solution {

    private TreeNode lastNode = null;
     
    public void flatten(TreeNode root) {
        if(root==null) return;
        if(lastNode != null){
        //connect current node to right of last visited node
          lastNode.left=null;
          lastNode.right=root;
        }
        lastNode=root;  //visit root, set it as last visited node
        TreeNode right=root.right;  //make a copy of right sub-tree, as root.right will be modified when flattening left sub-tree
        flatten(root.left);  //visit left sub-tree
        flatten(right);  //visit right sub-tree
        
    }
}
```

Version 2, divide & conquer

```java
public class Solution {

    public void flatten(TreeNode root) {
      flattenReturnLast(root);
    }
  
    //flatten and return last node
    private TreeNode flattenReturnLast(TreeNode root){
      if(root==null) return null;
      //flatten left sub-tree and right-subtree
      TreeNode leftLast = flattenReturnLast(root.left);
      TreeNode rightLast = flattenReturnLast(root.right);
      if(leftLast!=null){
        //connect flattened right to end of flattened left
        leftLast.right=root.right;
        //connect flattened left to right of root
        root.right = root.left;
        root.left=null;
      }
      if(rightLast!=null) return rightLast;
      if(leftLast!=null) return leftLast;
      return root;
    }
}
```

