# Partitioning & Sorting

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

时间复杂的为O\(Nlog\(k\)\)



