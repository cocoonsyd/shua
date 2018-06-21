# BFS on Graph

## Graph Valid Tree

[https://www.lintcode.com/problem/graph-valid-tree/](https://www.lintcode.com/problem/graph-valid-tree/)

Given n nodes labeled from 0 to n - 1 and a list of undirected edges \(each edge is a pair of nodes\), write a function to check whether these edges make up a valid tree.

一个有n个点组成的图是一个树的条件：

* 刚好n-1条边
* 所有点互相联通

  （可以证明如果满足上面两个条件，则一定无环）

```java
public class Solution {
    /**
     * @param n: An integer
     * @param edges: a list of undirected edges
     * @return: true if it's a valid tree, or false
     */
    public boolean validTree(int n, int[][] edges) {
        // write your code here
        if(n<=0) return false;
        if(edges.length!=n-1) return false;

        // translate graph into Adjacency List representation
        HashMap<Integer, HashSet<Integer>> graph = initializeGraph(n, edges);

        // BFS to check whether every node is connected to node 0
        Queue<Integer> queue = new LinkedList<Integer>();
        HashSet<Integer> hashset = new HashSet<Integer>();
        queue.add(0);
        hashset.add(0);
        while(!queue.isEmpty()){
            int curr = queue.remove();
            for(int adj : graph.get(curr)){
                if(!hashset.contains(adj)){
                    hashset.add(adj);
                    queue.add(adj);
                }
            }
        }
        return hashset.size()==n; // node 0 is connected to every node; meaning entire graph is connected
    }
    private HashMap<Integer, HashSet<Integer>> initializeGraph(int n, int[][] edges){
        HashMap<Integer, HashSet<Integer>> graph = new HashMap<Integer, HashSet<Integer>>();
        for(int i=0;i<n;i++){
            graph.put(i, new HashSet<Integer>());
        }
        for(int i=0;i<edges.length;i++){
            graph.get(edges[i][0]).add(edges[i][1]);
            graph.get(edges[i][1]).add(edges[i][0]);
        }
        return graph;
    }
}
```

## Clone Graph

### 方法1

Serialize然后de-serialize, 就可以得到原图的一个clone

### 方法2

(学习一种编程风格：“劝分不劝合”，将解决问题的过程分成几个小的步骤分别写，这样条例/逻辑更清晰)

分三步：

1. List all nodes \(BFS\) 

2. Clone nodes, save mapping from old node to new node

3. Clone edges

```java
/**
 * Definition for undirected graph.
 * class UndirectedGraphNode {
 *     int label;
 *     ArrayList<UndirectedGraphNode> neighbors;
 *     UndirectedGraphNode(int x) { label = x; neighbors = new ArrayList<UndirectedGraphNode>(); }
 * };
 */
 
public class Solution {
    /*
     * @param node: A undirected graph node
     * @return: A undirected graph node
     */
    public UndirectedGraphNode cloneGraph(UndirectedGraphNode node) {
        // write your code here
        if(node==null) return null;
        
        // use bfs to traverse the graph and get all nodes
        ArrayList<UndirectedGraphNode> allNodes = getAllNodes(node);
        
        // copy nodes, store the old->new mapping information in a hash map
        HashMap<UndirectedGraphNode, UndirectedGraphNode> mapping = new HashMap<>();
        for(UndirectedGraphNode oldNode : allNodes){
            UndirectedGraphNode newNode = new UndirectedGraphNode(oldNode.label);
            mapping.put(oldNode, newNode);
        }
        
        // copy neighbors(edges)
        for(UndirectedGraphNode oldNode : allNodes){
            UndirectedGraphNode newNode = mapping.get(oldNode);
            for(UndirectedGraphNode neighbor : oldNode.neighbors){
                newNode.neighbors.add(mapping.get(neighbor));
            }
        }
        
        return mapping.get(node);
    }
    
    private ArrayList<UndirectedGraphNode> getAllNodes(UndirectedGraphNode node){
        
        Queue<UndirectedGraphNode> queue = new LinkedList<UndirectedGraphNode>();
        HashSet<UndirectedGraphNode> hashset = new HashSet<UndirectedGraphNode>();
        
        queue.add(node);
        hashset.add(node);
        while(!queue.isEmpty()){
            UndirectedGraphNode cur = queue.remove();
            for(UndirectedGraphNode neighbor : cur.neighbors){
                if(!hashset.contains(neighbor)){
                    hashset.add(neighbor);
                    queue.add(neighbor);
                }
            }
        }
        
        return new ArrayList<UndirectedGraphNode>(hashset);
    }
}
```


