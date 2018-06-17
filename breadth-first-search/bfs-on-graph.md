# BFS on Graph
## Graph Valid Tree
https://www.lintcode.com/problem/graph-valid-tree/

Given n nodes labeled from 0 to n - 1 and a list of undirected edges (each edge is a pair of nodes), write a function to check whether these edges make up a valid tree.

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

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTU4NDkxODMzMCwtMzI3NTU5NTc4LDEyNz
EzMzc4OTUsLTcyNTU2MTUyMywtMTQ1MzMyMDgwOSwxNDU3Mjgx
MTY5LC01MjYyOTczOTFdfQ==
-->