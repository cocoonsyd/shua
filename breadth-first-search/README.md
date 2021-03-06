# Breadth First Search

#### BFS的两个使用条件：

1. 图的遍历（由点及面，从一个点找到连通的所有点，可以层级遍历）
2. 简单图最短路径

#### 是否需要层级遍历？

如果需要，加一句记录这一层点的数量：

int size=queue.size\(\)

#### 能用BFS解决的问题尽量用BFS，不要用DFS

因为用recursion实现的DFS可能会造成stack overflow，而non-recursion的DFS很难写也很难懂



## BFS的时间复杂度

### 图

n个点，m条边，则BFS的时间复杂度为O\(n+m\)

\(另外m最大为O\(n^2\)，所以BFS最坏时间复杂度为O\(n^2\)\)

### 矩阵

r行c列，r\*c个点，r\*c\*2条边

则BFS时间复杂度为O\(r\*c\)

