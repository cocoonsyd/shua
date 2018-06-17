# BFS on Graph
## test

test stackedit

```java
github test
```

```java

public class Solution {
    /*
     * @param root: The root of the binary search tree.
     * @param value: Remove the node with given value.
     * @return: The root of the binary search tree after removal.
     */
    public TreeNode removeNode(TreeNode root, int value) {
        // write your code here
        if(root==null) r
        if(value<root.val) root.left=removeNode(root.left, value);
        else if(value>root.val) root.right=removeNode(root.right, value);
        else{
            if(root.left==null) return root.right;
            if(root.right==null) return root.left;
            TreeNode minInRight=root.right;
            while(minInRight.left!=null) minInRight=minInRight.left;
            minInRight.right=removeNode(root.right, minInRight.val);
            minInRight.left=root.left;
            root=minInRight;
        }
        return root;
    }
}    
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQ1NzI4MTE2OSwtNTI2Mjk3MzkxXX0=
-->