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

[http://www.lintcode.com/problem/clone-graph/](http://www.lintcode.com/problem/clone-graph/)

### 方法1

Serialize然后de-serialize, 就可以得到原图的一个clone

### 方法2

\(学习一种编程风格：“劝分不劝合”，将解决问题的过程分成几个小的步骤分别写，这样条例/逻辑更清晰\)

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

## Topological Sorting

[https://www.lintcode.com/problem/topological-sorting/](https://www.lintcode.com/problem/topological-sorting/)

使用BFS做拓扑排序的步骤：

\(每次取入度为0的点，可以保证满足依赖关系，因为入度为0的点所依赖的点已经被拿出放到result里了，即result中一定是按照依赖关系排序\)

1. 统计所有点的入度（indegree）
2. 找出并记录所有入度为0的点
3. 从任意一个入度为0的点开始，每次拿出一个入度为0的点放到排序结果里，然后把它指向的所有点的入度减1，并把新产生的入度为0的点加到记录里

```java
/**
 * Definition for Directed graph.
 * class DirectedGraphNode {
 *     int label;
 *     ArrayList<DirectedGraphNode> neighbors;
 *     DirectedGraphNode(int x) { label = x; neighbors = new ArrayList<DirectedGraphNode>(); }
 * };
 */

public class Solution {
    /*
     * @param graph: A list of Directed graph node
     * @return: Any topological order for the given graph.
     */
    public ArrayList<DirectedGraphNode> topSort(ArrayList<DirectedGraphNode> graph) {
        // write your code here
        if(graph==null || graph.size()==0) return null;
        ArrayList<DirectedGraphNode> result = new ArrayList<>();

        // count indegree
        HashMap<DirectedGraphNode, Integer> indegree = new HashMap<>();
        for(DirectedGraphNode node : graph){
            for(DirectedGraphNode neighbor : node.neighbors){
                if(indegree.containsKey(neighbor)) indegree.put(neighbor, indegree.get(neighbor)+1);
                else indegree.put(neighbor, 1);
            }
        }

        // find nodes with indegree of 0
        Queue<DirectedGraphNode> indegree0 = new LinkedList<>();
        for(DirectedGraphNode node : graph){
            if(!indegree.containsKey(node)) indegree0.add(node);
        }

        // BFS from node with indegree 0, and update indegree & indegree0
        while(!indegree0.isEmpty()){
            DirectedGraphNode curr = indegree0.remove();
            result.add(curr);
            for(DirectedGraphNode node : curr.neighbors){
                int newIndegree = indegree.get(node)-1;
                indegree.put(node, newIndegree);
                if(newIndegree==0) indegree0.add(node);
            }
        }

        return result;
    }
}
```

## Course Schedule

[https://www.lintcode.com/problem/course-schedule](https://www.lintcode.com/problem/course-schedule)

```java
public class Solution {
    /*
     * @param numCourses: a total of n courses
     * @param prerequisites: a list of prerequisite pairs
     * @return: true if can finish all courses or false
     */
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        // write your code here
        HashMap<Integer, HashSet<Integer>> courseToPrereq = new HashMap<>();
        HashMap<Integer, HashSet<Integer>> prereqToCourse = new HashMap<>();
        
        for(int i=0;i<numCourses;i++){
            HashSet<Integer> init1 = new HashSet<>();
            HashSet<Integer> init2 = new HashSet<>();
            courseToPrereq.put(i,init1);
            prereqToCourse.put(i,init2);
        }
        for(int i=0;i<prerequisites.length;i++){
            int course=prerequisites[i][0];
            int prereq=prerequisites[i][1];
            if(!courseToPrereq.get(course).contains(prereq)) courseToPrereq.get(course).add(prereq);
            if(!prereqToCourse.get(prereq).contains(course)) prereqToCourse.get(prereq).add(course);
        }
        
        // find all courses with no prereqs
        Queue<Integer> prereqSatisfied = new LinkedList<Integer>();
        for(int i=0;i<numCourses;i++){
            if(courseToPrereq.get(i).size()==0) prereqSatisfied.add(i);
        }
        
        ArrayList<Integer> courseTaken = new ArrayList<>();
        
        while(!prereqSatisfied.isEmpty()){
            //take a course whose prereq is satisfied, and remove it from its dependants' prereq list
            int course = prereqSatisfied.remove();
            courseTaken.add(course);
            for(int dependant : prereqToCourse.get(course)){
                courseToPrereq.get(dependant).remove(course);
                if(courseToPrereq.get(dependant).size()==0) prereqSatisfied.add(dependant);
            }
        }
        return courseTaken.size()==numCourses;
    }
}
```

## Word Ladder

https://www.lintcode.com/problem/word-ladder/

```java
public class Solution {
    /*
     * @param start: a string
     * @param end: a string
     * @param dict: a set of string
     * @return: An integer
     */
    public int ladderLength(String start, String end, Set<String> dict) {
        // write your code here
        if(dict==null || dict.size()==0) return 0;
        if(start.equals(end)) return 1;
        
        dict.add(start);
        dict.add(end);
        
        HashSet<String> hash = new HashSet<>();
        Queue<String> queue = new LinkedList<>();
        hash.add(start);
        queue.add(start);
        
        int length = 1;
        while(!queue.isEmpty()){
            length++;
            int size = queue.size();
            for(int i=0;i<size;i++){
                String curr = queue.remove();
                for(String next : getNext(curr, dict)){
                    if(hash.contains(next)) continue;
                    else{
                        hash.add(next);
                        queue.add(next);
                    }
                    if(next.equals(end)) return length;
                }
            }
        }
        return 0;
    }
    
    private ArrayList<String> getNext(String curr, Set<String> dict){
        ArrayList<String> result = new ArrayList<>();
        for(char c='a';c<='z';c++){
            for(int i=0;i<curr.length();i++){
                if(c==curr.charAt(i)) continue;
                char[] charArray = curr.toCharArray();
                charArray[i]=c;
                String newString = new String(charArray);
                if(dict.contains(newString)) result.add(newString);
            }
        }
        return result;
    }
}
```


