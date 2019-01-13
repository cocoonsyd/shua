# 相向双指针

## Rotate String

[https://www.lintcode.com/problem/rotate-string](https://www.lintcode.com/problem/rotate-string)

注意如何将rotate操作分解为三步reverse操作

```java
public class Solution {

    public void rotateString(char[] str, int offset) {
        // write your code here
        if (str == null || str.length == 0)
            return;

        offset = offset % str.length;
        reverse(str, 0, str.length - offset - 1);
        reverse(str, str.length - offset, str.length - 1);
        reverse(str, 0, str.length - 1);
    }

    private void reverse(char[] str, int start, int end) {
        for (int i = start, j = end; i < j; i++, j--) {
            char temp = str[i];
            str[i] = str[j];
            str[j] = temp;
        }
    }
}
```

## Recover Rotated Sorted Array

[https://www.lintcode.com/problem/recover-rotated-sorted-array](https://www.lintcode.com/problem/recover-rotated-sorted-array)

注意如何将rotate操作分解为三步reverse操作

```java
public class Solution {
    /**
     * @param nums: An integer array
     * @return: nothing
     */
    private void rotate(List<Integer> nums, int start, int end){
        while(start<end){
            int tmp = nums.get(start);
            nums.set(start, nums.get(end));
            nums.set(end, tmp);
            start++;
            end--;
        }
    }

    public void recoverRotatedSortedArray(List<Integer> nums) {
        int index = 0;
        while(index<nums.size()-1 && nums.get(index+1)>=nums.get(index)){
            index++;
        }
        rotate(nums, 0, index);
        rotate(nums, index+1, nums.size()-1);
        rotate(nums, 0, nums.size()-1);
    }
}
```

