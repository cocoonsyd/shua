# Subarray

![](../.gitbook/assets/image%20%284%29.png)

## Subarray Sum

https://www.lintcode.com/problem/subarray-sum

```java
public class Solution {
    /**
     * @param nums: A list of integers
     * @return: A list of integers includes the index of the first number and the index of the last number
     */
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

