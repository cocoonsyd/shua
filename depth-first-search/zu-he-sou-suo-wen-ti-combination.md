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

https://www.lintcode.com/problem/subsets

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

...


