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



