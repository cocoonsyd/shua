# Binary Tree Traversal

## 二叉树前序遍历

非递归实现

```java
 public class Solution { public List<Integer> preorderTraversal(TreeNode root) { Stack<TreeNode> stack = new Stack<TreeNode>(); List<Integer> preorder = new ArrayList<Integer>(); if (root == null) { return preorder; } stack.push(root); while (!stack.empty()) { TreeNode node = stack.pop(); preorder.add(node.val); if (node.right != null) { stack.push(node.right); } if (node.left != null) { stack.push(node.left); } } return preorder; } }
```



