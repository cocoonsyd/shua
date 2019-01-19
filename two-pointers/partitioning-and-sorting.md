# Partitioning & Sorting

## Partition Array

[https://www.lintcode.com/problem/partition-array](https://www.lintcode.com/problem/partition-array)

类似quicksort

```java
public class Solution {

    public int partitionArray(int[] nums, int k) {
        // write your code here
        int lp=0;
        int rp=nums.length-1;

        while(lp<=rp){
            while(lp<=rp && nums[lp]<k){
                lp++;
            }
            while(lp<=rp && nums[rp]>=k){
                rp--;
            }
            if(lp<=rp){
                int tmp=nums[lp];
                nums[lp]=nums[rp];
                nums[rp]=tmp;
                lp++;
                rp--;
            }
        }

        return lp;
    }
}
```

## Sort Colors II

[https://www.lintcode.com/problem/sort-colors-ii](https://www.lintcode.com/problem/sort-colors-ii/description)

方法1: counting sort

```java
public class Solution {
    public void sortColors2(int[] colors, int k) {
        // write your code here
        int[] count = new int[k];
        for(int i : colors){
            count[i-1]++;
        }
        int p=0;
        for(int i=0;i<k;i++){
            for(int j=0;j<count[i];j++){
                colors[p]=i+1;
                p++;
            }
        }
    }
}
```

方法2: rainbow sort

时间复杂的为O\(nlog\(k\)\)

```java
public class Solution {

    public void sortColors2(int[] colors, int k) {
        rainbowSort(colors, 0, colors.length-1, 1, k);
    }

    private void rainbowSort(int[] colors, int left, int right, int colorFrom, int colorTo){

        if(colorFrom>=colorTo) return;
        if(left>=right) return;

        int colorMid=(colorFrom+colorTo)/2;
        int leftPointer=left;
        int rightPointer=right;
        while(leftPointer<=rightPointer){
            while(leftPointer<=right && colors[leftPointer]<=colorMid){
                leftPointer++;
            }
            while(rightPointer>=left && colors[rightPointer]>colorMid){
                rightPointer--;
            }
            if(leftPointer<=rightPointer){
                int tmp=colors[leftPointer];
                colors[leftPointer]=colors[rightPointer];
                colors[rightPointer]=tmp;
                leftPointer++;
                rightPointer--;
            }
        }

        rainbowSort(colors, left, leftPointer, colorFrom, colorMid);
        rainbowSort(colors, rightPointer, right, colorMid+1, colorTo);
    }
}
```

