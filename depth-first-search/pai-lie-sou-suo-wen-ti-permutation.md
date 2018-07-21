# 排列搜索问题 Permutation

问题模型：求出所有满足条件的“排列”

判断条件：组合中的元素是顺序“相关”的

时间复杂度：与 n! 相关

## Permutations

https://www.lintcode.com/problem/permutations/

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

https://www.lintcode.com/problem/permutations-ii

重点在于如何去重。和没有重复元素的 Permutations 一题相比，只加了两句话：

* Arrays.sort(nums) // 排序，把所有重复的数凑到一起

* if (i > 0 && nums[i] == nums[i - 1] && !visited[i - 1]) { continue; } // 跳过会造成重复的情况

```java
public class Solution {
    /*
     * @param :  A list of integers
     * @return: A list of unique permutations
     */
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
            if (i > 0 && nums[i] == nums[i - 1] && !visited[i - 1]) {
                continue;
            }
            permutation.add(nums[i]);
            visited[i]=true;
            helper(nums, permutation, visited, result);
            permutation.remove(permutation.size()-1);
            visited[i]=false;
        }
    }
}
```java

