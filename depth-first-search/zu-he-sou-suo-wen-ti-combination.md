# 组合搜索问题 Combination

问题模型：求出所有满足条件的“组合”

判断条件：组合中的元素是顺序无关的

时间复杂度：与 2^n 相关

## 递归三要素

一般来说，如果面试官不特别要求的话，DFS都可以使用递归\(Recursion\)的方式来实现。

递归三要素是实现递归的重要步骤：

• 递归的定义

• 递归的拆解

• 递归的出口

## Subsets

[https://www.lintcode.com/problem/subsets](https://www.lintcode.com/problem/subsets)

```java
public class Solution {

    public List<List<Integer>> subsets(int[] nums) {
        // write your code here
        List<List<Integer>> results = new ArrayList<>();
        if(nums==null) return results;
        if(nums.length==0){
            results.add(new ArrayList<Integer>());
            return results;
        }
        Arrays.sort(nums);
        helper(new ArrayList<Integer>(), nums, 0, results);
        return results;
    }

    // 递归三要素
    // 1. 递归的定义：在 Nums 中找到所有以 subset 开头的的集合，并放到 results
    private void helper(ArrayList<Integer> subset, int[] nums, int startIndex, List<List<Integer>> results){

        // 2. 递归的拆解
        //need to use deep copy, cannot just results.add(subset)
        results.add(new ArrayList<Integer>(subset));
        for(int i=startIndex;i<nums.length;i++){
            subset.add(nums[i]);
            helper(subset, nums, i+1, results);
            subset.remove(subset.size()-1);
        }

        // 3. 递归的出口
        // return;
    }
}
```

## Combination Sum

[https://www.lintcode.com/problem/combination-sum](https://www.lintcode.com/problem/combination-sum)

```java
public class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        // write your code here
        List<List<Integer>> results = new ArrayList<>();
        if(candidates==null || candidates.length==0) return results;
        Arrays.sort(candidates);
        helper(new ArrayList<Integer>(), candidates, target, 0, results);
        return results;
    }
    //递归的定义，找到所有以combination开头的组合，且后面的和为target
    private void helper(ArrayList<Integer> combinations, int[] candidates, int target, int startIndex, List<List<Integer>> results){
        //递归的出口
        if(target==0){
            //这里要用deep copy
            results.add(new ArrayList<Integer>(combinations));
            return;
        }

        if(target<0) return;

        //递归的拆解
        for(int i=startIndex; i<candidates.length; i++){
            //去除candidates数组中可能的重复
            if(i!=0 && candidates[i]==candidates[i-1]) continue;
            combinations.add(candidates[i]);
            //这里startIndex从i开始而不是i+1，因为题目要求里说允许重复
            helper(combinations, candidates, target-candidates[i], i, results);
            combinations.remove(combinations.size()-1);
        }
    }
}
```

## Palindrome Partitioning

[https://www.lintcode.com/problem/palindrome-partitioning/](https://www.lintcode.com/problem/palindrome-partitioning/)

找一个字符串所有可能的partitioning可以理解为找切割点的所有组合

一个长度为n的字符串有n-1个可能的切割点

```java
public class Solution {
    public List<List<String>> partition(String s) {
        // write your code here
        List<List<String>> result = new ArrayList<>();
        if(s==null || s.length()==0) return result;
        helper(s, 0, new ArrayList<String>(), result);
        return result;
    }

    private void helper(String s, int startIndex, ArrayList<String> partition, List<List<String>> result){
        if(startIndex==s.length()){
            result.add(new ArrayList<String>(partition));
            return;
        }

        for(int i=startIndex; i<s.length(); i++){
            String subString = s.substring(startIndex, i+1);
            if(!isPalindrome(subString)) continue;
            partition.add(subString);
            helper(s, i+1, partition, result);
            partition.remove(partition.size()-1);
        }
    }

    private boolean isPalindrome(String s){
        for(int i=0, j=s.length()-1; i<j; i++,j--){
            if(s.charAt(i)!=s.charAt(j)) return false;
        }
        return true;
    }
}
```

