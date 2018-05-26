# Binary Tree Traversal

## 二叉树前序遍历

[http://www.lintcode.com/problem/binary-tree-preorder-traversal/](http://www.lintcode.com/problem/binary-tree-preorder-traversal/)

非递归实现

```java
/**
 * Definition of TreeNode:
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left, right;
 *     public TreeNode(int val) {
 *         this.val = val;
 *         this.left = this.right = null;
 *     }
 * }
 */
public class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        if(root==null) return null;
        ArrayList<Integer> result = new ArrayList<Integer>();
        Stack<TreeNode> stack = new Stack<TreeNode>();
        stack.push(root);
        while(!stack.isEmpty()){
            TreeNode r = stack.pop();
            result.add(r.val);
            if(r.right!=null) stack.push(r.right);
            if(r.left!=null) stack.push(r.left);
        }
        return result;
    }
}
```

## 二叉树中序遍历

[https://www.lintcode.com/problem/binary-tree-inorder-traversal/](https://www.lintcode.com/problem/binary-tree-inorder-traversal/)

非递归实现

　　根据中序遍历的顺序，对于任一结点，优先访问其左孩子，而左孩子结点又可以看做一根结点，然后继续访问其左孩子结点，直到遇到左孩子结点为空的结点才进行访问，然后按相同的规则访问其右子树。因此其处理过程如下：

　　对于任一结点P，

  　1\)若其左孩子不为空，则将P入栈并将P的左孩子置为当前的P，然后对当前结点P再进行相同的处理；

 　 2\)若其左孩子为空，则取栈顶元素并进行出栈操作，访问该栈顶结点，然后将当前的P置为栈顶结点的右孩子；

  　3\)直到P为NULL并且栈为空则遍历结束。

```java
public class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        if(root==null) return null;
        ArrayList<Integer> result = new ArrayList<Integer>();
        Stack<TreeNode> stack = new Stack<TreeNode>();
        TreeNode current = root;
        while(current!=null || !stack.isEmpty()){
            while(current!=null) {
                stack.push(current);
                current=current.left;
            }
            current=stack.pop();
            result.add(current.val);
            current=current.right;
        }
        return result;
    }
}
```

## 二叉树后序遍历

[https://www.lintcode.com/problem/binary-tree-postorder-traversal/](https://www.lintcode.com/problem/binary-tree-postorder-traversal/)

非递归实现



