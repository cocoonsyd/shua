# BFS on Binary Tree

## Binary Tree Level Order Traversal

[https://www.lintcode.com/problem/binary-tree-level-order-traversal/](https://www.lintcode.com/problem/binary-tree-level-order-traversal/)

Given a binary tree, return the level order traversal of its nodes' values. \(ie, from left to right, level by level\).

![](../.gitbook/assets/screenshot-2018-06-16-at-11.15.49-am.png)

## Serialize and Deserialize Binary Tree

[https://www.lintcode.com/problem/binary-tree-serialization/](https://www.lintcode.com/problem/binary-tree-serialization/)

Design an algorithm and write code to serialize and deserialize a binary tree. Writing the tree to a file is called 'serialization' and reading back from the file to reconstruct the exact same binary tree is 'deserialization'.

![](../.gitbook/assets/image%20%282%29.png)

## Binary Tree Zigzag Level Order Traversal

[https://www.lintcode.com/problem/binary-tree-zigzag-level-order-traversal/](https://www.lintcode.com/problem/binary-tree-zigzag-level-order-traversal/)

Given a binary tree, return the zigzag level order traversal of its nodes' values. \(ie, from left to right, then right to left for the next level and alternate between\).

该方法利用数据结构双向队列的特点，利用从前端读，从后端读，从前端写，从后端写的交替使用，保证每层数据有序的进入队列中；算法时间复杂度为O\(n\)

![](../.gitbook/assets/image%20%283%29.png)

