# 排列搜索问题 Permutation

问题模型：求出所有满足条件的“排列”

判断条件：组合中的元素是顺序“相关”的

时间复杂度：与 n! 相关

## Permutations

[https://www.lintcode.com/problem/permutations/](https://www.lintcode.com/problem/permutations/)

```java
public class Solution {
    public List<List<Integer>> permute(int[] nums) {
        // write your code here
        List<List<Integer>> result = new ArrayList<>();
        if(nums==null) return result;

        helper(nums, new ArrayList<Integer>(), new boolean[nums.length], result);
        return result;
    }

    //find all permutations starting with ArrayList "permutation", visited elements in "permutation" are marked in "visited" array
    private void helper(int[] nums, ArrayList<Integer> permutation, boolean[] visited, List<List<Integer>> result){
        if(permutation.size()==nums.length){
            result.add(new ArrayList<Integer>(permutation));
            return;
        }
        for(int i=0; i<nums.length; i++){
            if(visited[i]) continue;
            permutation.add(nums[i]);
            visited[i]=true;
            helper(nums, permutation, visited, result);
            permutation.remove(permutation.size()-1);
            visited[i]=false;
        }
    }
}
```

## Permutations II

[https://www.lintcode.com/problem/permutations-ii](https://www.lintcode.com/problem/permutations-ii)

重点在于如何去重。和没有重复元素的 Permutations 一题相比，只加了两句话：

* Arrays.sort\(nums\) // 排序，把所有重复的数凑到一起
* if \(i &gt; 0 && nums\[i\] == nums\[i - 1\] && !visited\[i - 1\]\) { continue; } // 跳过会造成重复的情况

```java
public class Solution {

    public List<List<Integer>> permuteUnique(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        if(nums==null) return result;
        Arrays.sort(nums);
        helper(nums, new ArrayList<Integer>(), new boolean[nums.length], result);
        return result;
    }

    private void helper(int[] nums, ArrayList<Integer> permutation, boolean[] visited, List<List<Integer>> result){
        if(permutation.size()==nums.length){
            result.add(new ArrayList<Integer>(permutation));
            return;
        }
        for(int i=0; i<nums.length; i++){
            if(visited[i]) continue;
            // 去重，即不允许跳过前一个相同的数字访问后一个数字。
            if (i > 0 && nums[i] == nums[i - 1] && !visited[i - 1]) continue;
            permutation.add(nums[i]);
            visited[i]=true;
            helper(nums, permutation, visited, result);
            permutation.remove(permutation.size()-1);
            visited[i]=false;
        }
    }
}
```

## N-Queens

[https://www.lintcode.com/problem/n-queens](https://www.lintcode.com/problem/n-queens)

```java
public class Solution {
    /*
     * @param n: The number of queens
     * @return: All distinct solutions
     */
    public List<List<String>> solveNQueens(int n) {
        // write your code here
        ArrayList<ArrayList<Integer>> result = new ArrayList<ArrayList<Integer>>();

        helper(new ArrayList<Integer>(), n, result);

        //convert result to chessboard
        List<List<String>> chessboards = new ArrayList<>();
        for(ArrayList<Integer> solution : result){
            ArrayList<String> chessboard = new ArrayList<>();
            for (int i = 0; i < solution.size(); i++) {
            StringBuilder sb = new StringBuilder();
            for (int j = 0; j < solution.size(); j++) {
                sb.append(j == solution.get(i) ? 'Q' : '.');
            }
            chessboard.add(sb.toString());
         }
        chessboards.add(chessboard);
        }
        return chessboards;  
    }

    //DFS
    private void helper(ArrayList<Integer> resultSoFar, int n, ArrayList<ArrayList<Integer>> result){
        if(resultSoFar.size()==n){
            result.add(new ArrayList<Integer>(resultSoFar));
            return;
        }
        for(int i=0;i<n;i++){
            if(!isValid(resultSoFar,i)) continue;
            resultSoFar.add(i);
            helper(resultSoFar, n, result);
            resultSoFar.remove(resultSoFar.size()-1);
        }
    }

    //check new queen doesn't conflict with existing queens
    private boolean isValid(ArrayList<Integer> resultSoFar, int cur){
        for(int i=0;i<resultSoFar.size();i++){
            if(cur==resultSoFar.get(i)) return false;
            if(i+resultSoFar.get(i)==cur+resultSoFar.size()) return false;
            if(i-resultSoFar.get(i)==resultSoFar.size()-cur) return false;
        }
        return true;
    }
}
```

## Word Ladder II

[https://www.lintcode.com/problem/word-ladder-ii/](https://www.lintcode.com/problem/word-ladder-ii/)

与Word Ladder对比，Word Ladder II不仅要找出最短路径的长度，还要列出所有的最短路径，所以需要BFS与DFS的结合

先BFS计算出每个node到终点的距离，然后从起点到终点做DFS，保证每走一步都离终点更近了（保证最短路径）

```java
public class Solution {

    public List<List<String>> findLadders(String start, String end, Set<String> dict) {

        List<List<String>> result = new ArrayList<>();
        HashMap<String, Integer> distToEnd = new HashMap<>();

        dict.add(start);
        dict.add(end);

        bfs(dict, end, distToEnd);
        dfs(dict, end, distToEnd, start, new ArrayList<String>(), result);

        return result;
    }

    private void bfs(Set<String> dict, String end, HashMap<String, Integer> distToEnd){
        Queue<String> queue = new LinkedList<>();
        int dist=0;
        queue.add(end);
        distToEnd.put(end,dist);
        while(!queue.isEmpty()){
            dist++;
            int size=queue.size();
            for(int i=0;i<size;i++){
                String curr = queue.remove();
                for(String next : nextWords(curr,dict)){
                    if(!distToEnd.containsKey(next)) {
                        queue.add(next);
                        distToEnd.put(next,dist);
                    }
                }
            }
        }
    }

    private void dfs(Set<String> dict, String end, HashMap<String, Integer> distToEnd, String curr, List<String> path, List<List<String>> result){

        path.add(curr);

        if(curr.equals(end)){
            // 注意这里需要用deep copy！不能直接result.add(path)！
            result.add(new ArrayList<String>(path));
        }
        else{
            for(String next : nextWords(curr, dict)){
                if(distToEnd.containsKey(next) && distToEnd.get(next)==distToEnd.get(curr)-1){
                    dfs(dict, end, distToEnd, next, path, result);
                }
            }
        }

        path.remove(path.size()-1);
    }

    private List<String> nextWords(String word, Set<String> dict){
        List<String> result = new ArrayList<>();
        for(int i=0;i<word.length();i++){
            for(char c='a';c<='z';c++){
                if(c!=word.charAt(i)){
                    String newWord = word.substring(0,i) + c + word.substring(i+1);
                    if(dict.contains(newWord)) result.add(newWord);
                }
            }
        }
        return result;
    }
}
```

