# Medium

## 5. Longest Palindromic Substring

Method 1: Dynamic programming:

O\(n^2\) time and O\(n^2\) space

```text
class Solution {
    public String longestPalindrome(String s) {
        if(s.length()==0) return new String();
        int n = s.length();
        boolean[][] dp = new boolean[n][n];
        
        int maxLength = 1;
        int start = 0;
        
        for(int i=0;i<n;i++){
            dp[i][i] = true;
        }
        for(int i=0;i+1<n;i++){
            if(s.charAt(i)==s.charAt(i+1)){
                dp[i][i+1] = true;
                maxLength = 2;
                start = i;
            }
        }
        for(int length=3;length<=n;length++){
            for(int i=0;i+length<=n;i++){
                int j=i+length-1;
                if(s.charAt(i)==s.charAt(j)&&dp[i+1][j-1]){
                    dp[i][j] = true;
                    if(length>maxLength){
                        start=i;
                        maxLength=length;
                    }
                }
            }
        }
        //System.out.println(start);
        //System.out.println(maxLength);
        return s.substring(start,start+maxLength);
        
    }
}
```

Method 2: Expand from center, iterate through all center locations.

O\(n^2\) time and O\(1\) space



