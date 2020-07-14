# Problem
Lintcode 110. Minimum Path Sum

# Problem description
Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right which minimizes the sum of all numbers along its path.



**Example**:
```
Example 1:
	Input:  [[1,3,1],[1,5,1],[4,2,1]]
	Output: 7
	
	Explanation:
	Path is: 1 -> 3 -> 1 -> 1 -> 1
```

```
Example 2:
	Input:  [[1,3,2]]
	Output: 6
	
	Explanation:  
	Path is: 1 -> 3 -> 2
```

**Notice**:
```
You can only go right or down in the path..
```
# Tool kits
Dynamic programming

# Thoughts
**Status**: Use f[i][j] to represent the sum of numbers on the route from (0, 0) to (i, j). Set coordinates at right-bottom as f[n -1][m - 1] and the last step must be from f[n - 2][m - 1] or f[n - 1][m - 2] <br/><br/>

**trasfer equation**: f[i][j] = min{f[i - 1][j], f[i][j - 1]} + grid[i][j] <br/><br/>

**starting status and border**: f[0][0] = grid[0][0]. When one of i or j equals 0, the current point is from either left or up. <br/><br/>

**calculating sequence**: f[0][0...n-1], f[1][0,...n-1] ...

# Difficulties:
How to save space? <br/> 
Solution: Use rolling array. Since each f[i][j] can be calculated from f[i - 1][j] or f[i][j - 1], we only need to store the last value and curretn value of i. Therefore, we can mod 2 to each of f[i] to save space. But we can only do it for one of i or j, not both.

# Code (JAVA)
```java
public class Solution {
    /**
     * @param grid: a list of lists of integers
     * @return: An integer, minimizes the sum of all numbers along its path
     */
    public int minPathSum(int[][] grid) {
        // write your code here
        int n = grid.length;
        int m = grid[0].length;
        
        // f stores the sum of number at each point
        int[][] f = new int[2][m];
        
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                // top left
                if (i == 0 && j == 0) {
                    f[i % 2][j] = grid[0][0];
                    continue;
                }
                
                // no up, can only be from left
                if (i == 0) {
                    f[i % 2][j] = f[i % 2][j - 1] + grid[i][j];
                    continue;
                }
                
                // no left, can only be from up
                if (j == 0) {
                    f[i % 2][j] = f[(i - 1) % 2][j] + grid[i][j];
                    continue;
                }
                
                // has both left and up
                f[i % 2][j] = Math.min(f[(i - 1) % 2][j], f[i % 2][j - 1]) + grid[i][j];
            }
        }
        
        return f[(n - 1) % 2][m - 1];
    }
}
```

# Complexity Analysis
**Time Complexity**: O(n * m)

**Space Complexity**: O(n * m)

# Follow Up
