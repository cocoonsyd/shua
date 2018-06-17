# BFS on Graph

```java
public class Solution {
public List<List<Integer>> zigzagLevelOrder(TreeNode root) {    // write your code here    List<List<Integer>> result = new ArrayList<List<Integer>>();    if(root==null) return result;    LinkedList<TreeNode> queue = new LinkedList<TreeNode>();    queue.add(root);    boolean left2right = false;    while(!queue.isEmpty()){        int size = queue.size();        List<Integer> current_level = new ArrayList<Integer>();        for(int i=0;i<size;i++){            if(left2right){                TreeNode curr = queue.removeFirst();                current_level.add(curr.val);                if(curr.right!=null) queue.addLast(curr.right);                if(curr.left!=null) queue.addLast(curr.left);            }            else{                TreeNode curr = queue.removeLast();                current_level.add(curr.val);                if(curr.left!=null) queue.addFirst(curr.left);                if(curr.right!=null) queue.addFirst(curr.right);            }        }        left2right = !left2right;        result.add(current_level);    }    return result;}
}
```



```text
public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
    // write your code here
    List<List<Integer>> result = new ArrayList<List<Integer>>();
    if(root==null) return result;
    LinkedList<TreeNode> queue = new LinkedList<TreeNode>();
    queue.add(root);
    boolean left2right = false;
    while(!queue.isEmpty()){
        int size = queue.size();
        List<Integer> current_level = new ArrayList<Integer>();
        for(int i=0;i<size;i++){
            if(left2right){
                TreeNode curr = queue.removeFirst();
                current_level.add(curr.val);
                if(curr.right!=null) queue.addLast(curr.right);
                if(curr.left!=null) queue.addLast(curr.left);
            }
            else{
                TreeNode curr = queue.removeLast();
                current_level.add(curr.val);
                if(curr.left!=null) queue.addFirst(curr.left);
                if(curr.right!=null) queue.addFirst(curr.right);
            }
        }
        left2right = !left2right;
        result.add(current_level);
    }
    return result;
}
```

```text
public class Solution { public List> zigzagLevelOrder(TreeNode root) { // write your code here List> result = new ArrayList>(); if(root==null) return result; LinkedList queue = new LinkedList(); queue.add(root); boolean left2right = false; while(!queue.isEmpty()){ int size = queue.size(); List current_level = new ArrayList(); for(int i=0;i<size;i++){ if(left2right){ TreeNode curr = queue.removeFirst(); current_level.add(curr.val); if(curr.right!=null) queue.addLast(curr.right); if(curr.left!=null) queue.addLast(curr.left); } else{ TreeNode curr = queue.removeLast(); current_level.add(curr.val); if(curr.left!=null) queue.addFirst(curr.left); if(curr.right!=null) queue.addFirst(curr.right); } } left2right = !left2right; result.add(current_level); } return result; }
}
```

