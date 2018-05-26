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

    要保证根结点在左孩子和右孩子访问之后才能访问，因此对于任一结点P，先将其入栈。如果P不存在左孩子和右孩子，则可以直接访问它；或者P存 在左孩子或者右孩子，但是其左孩子和右孩子都已被访问过了，则同样可以直接访问该结点。若非上述两种情况，则将P的右孩子和左孩子依次入栈，这样就保证了 每次取栈顶元素的时候，左孩子在右孩子前面被访问，左孩子和右孩子都在根结点前面被访问。

```java
public class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<Integer>();
        if(root==null) return result;
        Stack<TreeNode> stack = new Stack<TreeNode>();
        TreeNode prev = null;
        TreeNode curr = root;
        stack.push(root);
        while(!stack.isEmpty()){
            curr = stack.peek();
            if(curr.left==null && curr.right==null){  //curr doesn't have child, access curr
                result.add(curr.val);
                prev = curr;
                stack.pop();
            }
            else if(prev!=null && (prev==curr.left || prev==curr.right)){  //curr has child but child has been accessed already, access curr
                result.add(curr.val);
                prev = curr;
                stack.pop();
            }
            else{  //curr has child that hasn't been accessed first
                if(curr.right!=null) stack.push(curr.right);
                if(curr.left!=null) stack.push(curr.left);
            }
        }
        return result;
    }
}
```



