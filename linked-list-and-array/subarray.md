# Subarray

![](../.gitbook/assets/image%20%284%29.png)

## Maximum Subarray

https://www.lintcode.com/problem/maximum-subarray

```java
public class Solution {
    public int maxSubArray(int[] nums) {
        // write your code here
        int i=0;
        int max=Integer.MIN_VALUE;
        int sum=0;
        while(i<nums.length){
            sum=sum+nums[i];
            max=Math.max(max,sum);
            if(sum<0) sum=0;
            i++;
        }
        return max;
    }
}
```

## Subarray Sum

https://www.lintcode.com/problem/subarray-sum

```java
public class Solution {
    public List<Integer> subarraySum(int[] nums) {
        // write your code here
        List<Integer> result = new ArrayList<>();
        HashMap<Integer,Integer> map = new HashMap<>();
        int sum=0;
        map.put(0,-1);
        for(int i=0;i<nums.length;i++){
            sum=sum+nums[i];
            if(map.containsKey(sum)){
                result.add(map.get(sum)+1);
                result.add(i);
                return result;
            }
            
            map.put(sum,i);
            
        }
        return result;
    }
}
```

