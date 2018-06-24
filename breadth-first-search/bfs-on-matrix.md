# BFS on Matrix

矩阵就理解为一个图，矩阵中每个点可能邻接的点是上下左右四个点

## Number of Islands

https://www.lintcode.com/problem/number-of-islands/

矩阵中的BFS，注意(1)check in boundary (2)坐标变换数组的用法

另外可以新建一个return type class来表示坐标，这样就只用一个queue了

https://www.jiuzhang.com/solutions/number-of-islands/

```java
public class Solution {
    /**
     * @param grid: a boolean 2D matrix
     * @return: an integer
     */
    public int numIslands(boolean[][] grid) {
        // write your code here
        if(grid==null || grid.length==0 || grid[0].length==0) return 0;
        int height=grid.length;
        int width=grid[0].length;
        int result=0;
        for(int i=0;i<height;i++){
            for(int j=0;j<width;j++){
                if(grid[i][j]){
                    markByBFS(grid,j,i);
                    result++;
                }
            }
        }
        return result;
    }
    
    private void markByBFS(boolean[][] grid, int x, int y){
        // 坐标变换数组
        int[] deltaX = {1,-1,0,0};
        int[] deltaY = {0,0,1,-1};
        
        Queue<Integer> qX = new LinkedList<>();
        Queue<Integer> qY = new LinkedList<>();
        qX.add(x);
        qY.add(y);
        
        // BFS to mark all neighboring land
        while(!qX.isEmpty()){
            int currX=qX.remove();
            int currY=qY.remove();
            grid[currY][currX]=false;
            for(int i=0;i<4;i++){
                if(inBound(grid, currX+deltaX[i], currY+deltaY[i]) && grid[currY+deltaY[i]][currX+deltaX[i]]){
                    qX.add(currX+deltaX[i]);
                    qY.add(currY+deltaY[i]);
                }
            }
        }
    }
    
    private boolean inBound(boolean[][] grid, int x, int y){
        int height=grid.length;
        int width=grid[0].length;
        return x>=0 && y>=0 && x<width && y<height;
    }
}
```
