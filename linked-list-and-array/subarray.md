# Subarray

![](../.gitbook/assets/image%20%284%29.png)

## Maximum Subarray

[https://www.lintcode.com/problem/maximum-subarray](https://www.lintcode.com/problem/maximum-subarray)

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

[https://www.lintcode.com/problem/subarray-sum](https://www.lintcode.com/problem/subarray-sum)

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

## Subarray Sum Closest

[https://www.lintcode.com/problem/subarray-sum-closest](https://www.lintcode.com/problem/subarray-sum-closest)

首先O\(n^2\)的算法很好想，直接枚举起点就行，看到挑战的复杂度，想肯定要排序或者二分什么的，这里没找出能二分的性质来，所以想只能想排序了，我们知道连续数组的和其实就是前缀和之间的差，而要求和最接近于零，也就是说，两个前缀和要最接近，那么直接前缀和排序，相邻两项比较即可。

```java
class Pair{
    int index;
    int sum;
    public Pair(int index, int sum){
        this.index=index;
        this.sum=sum;
    }

}

public class Solution {
    /*
     * @param nums: A list of integers
     * @return: A list of integers includes the index of the first number and the index of the last number
     */
    public int[] subarraySumClosest(int[] nums) {
        // write your code here
        int[] result = new int[2];
        if(nums==null || nums.length==0) return result;
        if(nums.length==1){
            result[0]=0;
            result[1]=0;
            return result;
        }
        Pair[] prefixSum = new Pair[nums.length+1];
        prefixSum[0] = new Pair(0,0);
        for(int i=0;i<nums.length;i++){
            prefixSum[i+1]=new Pair(i+1, prefixSum[i].sum+nums[i]);
        }
        Arrays.sort(prefixSum, new Comparator<Pair>(){
                public int compare(Pair a, Pair b){
                    return a.sum-b.sum;
                }
        });
        int minDiff=Integer.MAX_VALUE;
        for(int i=1;i<prefixSum.length;i++){
            if(minDiff>prefixSum[i].sum - prefixSum[i-1].sum){
                minDiff=prefixSum[i].sum-prefixSum[i-1].sum;
                result[0]=prefixSum[i].index;
                result[1]=prefixSum[i-1].index;
                Arrays.sort(result);
                result[1]--;
            }
        }
        return result;
    }
}

/*
问：为什么需要一个 (0,0) 的初始 Pair?
答：
我们首先需要回顾一下，在 subarray 这节课里，我们讲过一个重要的知识点，叫做 Prefix Sum
比如对于数组 [1,2,3,4]，他的 Prefix Sum 是 [1,3,6,10]
分别表示 前1个数之和，前2个数之和，前3个数之和，前4个数之和
这个时候如果你想要知道 子数组 从下标  1 到下标 2 的这一段的和(2+3)，就用前 3个数之和 减去 前1个数之和 = PrefixSum[2] - PrefixSum[0] = 6 - 1 = 5
你可以看到这里的 前 x 个数，和具体对应的下标之间，存在 +-1 的问题
第 x 个数的下标是 x - 1，反之 下标 x 是第 x + 1 个数
那么问题来了，如果要计算 下标从 0~2 这一段呢？也就是第1个数到第3个数，因为那样会访问到 PrefixSum[-1]
所以我们把 PrefixSum 整体往后面移动一位，把第0位空出来表示前0个数之和，也就是0. => [0,1,3,6,10]
那么此时就用 PrefixSum[3] - PrefixSum[0] ，这样计算就更方便了。
此时，PrefixSum[i] 代表 前i个数之和，也就是 下标区间在 0 ~ i-1 这一段的和

那么回过头来看看，为什么我们需要一个 (0,0) 的 pair 呢？
因为 这个 0,0 代表的就是前0个数之和为0
一个 n 个数的数组， 变成了 prefix Sum 数组之后，会多一个数出来
*/
```

