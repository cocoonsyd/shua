# Binary Tree

## Divide & Conquer 思路

碰到二叉树的问题，就想想整棵树在该问题上的结果和左右儿子在该问题上的结果之间的联系是什么



## Traverse // Divide & Conquer // Non-Recursion

![](../.gitbook/assets/image.png)

####  Traverse vs Divide Conquer

• They are both Recursion Algorithm

• Result in parameter vs Result in return value

• Top down vs Bottom up



P.S. **关于使用递归解题**：使用递归的时候注意递归深度不能太深，一道题如果用递归做会导致递归深度很深，最好想别的办法。比如链表题通常不要用递归做。



#### Example: 二叉树的前序遍历

Traverse\(注意返回值为空\)

```java
//Version 1: Traverse
public class Solution {
    public ArrayList<Integer> preorderTraversal(TreeNode root) {
        ArrayList<Integer> result = new ArrayList<Integer>();
        traverse(root, result);
        return result;
    }
    private void traverse(TreeNode root, ArrayList<Integer> result) {
        if (root == null) {
            return;
        }
        result.add(root.val);
        traverse(root.left, result);
        traverse(root.right, result);
    }
}
```

Divide & Conquer（注意返回值非空）

```java
//Version 2: Divide & Conquer
public class Solution {
    public ArrayList<Integer> preorderTraversal(TreeNode root) {
        ArrayList<Integer> result = new ArrayList<Integer>();
        // null or leaf
        if (root == null) return result;
        // Divide
        ArrayList<Integer> left = preorderTraversal(root.left);
        ArrayList<Integer> right = preorderTraversal(root.right);
        // Conquer
        result.add(root.val);
        result.addAll(left);
        result.addAll(right);
        return result;
    }
}
```



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



