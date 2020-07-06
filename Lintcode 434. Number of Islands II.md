# Problem
434. Number of Islands II

# Problem description
Given a n,m which means the row and column of the 2D matrix and an array of pair A( size k). Originally, the 2D matrix is all 0 which means there is only sea in the matrix. The list pair has k operator and each operator has two integer A[i].x, A[i].y means that you can change the grid matrix[A[i].x][A[i].y] from sea to island. Return how many island are there in the matrix after each operator.



**Example**:
```
Input: n = 4, m = 5, A = [[1,1],[0,1],[3,3],[3,4]]
Output: [1,1,2,2]
Explanation:
0.  00000
    00000
    00000
    00000
1.  00000
    01000
    00000
    00000
2.  01000
    01000
    00000
    00000
3.  01000
    01000
    00000
    00010
4.  01000
    01000
    00000
    00011
```

```
Input: n = 3, m = 3, A = [[0,0],[0,1],[2,2],[2,1]]
Output: [1,1,2,2]
```

**Notice**:
```
0 is represented as the sea, 1 is represented as the island. If two 1 is adjacent, we consider them in the same island. We only consider up/down/left/right adjacent.
```
# Tool kits
Union find

# Thoughts
DFS is not a good solution here, because the grid is not static. Operations will be conducted one by one and thus this is a dynamic process. If only use DFS, the whole grid will be searched after each operation and the time complexity will be n*m*k (k stands for the number of operations).This is too much.<br/><br/> Union find can deal with such dynamic question more elegently because each block of connected islands will be represented by only one bigBrother. Thus, there is no need to search the whole grid again. 

# Difficulties:
1. how to union-find points that are in a 2D-matrix? <br/><br/> Solution: Compress point to int by using x * m + y to represent a point. <br/><br/> 
2. how to adjust size? <br/><br/> If the operator is not an existing island, firstly regard it as an isolated one and keep on searching its adjacent tiles to see if there are islands. Union the operator with its adjacent islands if there are any and each time the union is implemented, decrease the size by 1.

# Code (JAVA)
```java
/**
 * Definition for a point.
 * class Point {
 *     int x;
 *     int y;
 *     Point() { x = 0; y = 0; }
 *     Point(int a, int b) { x = a; y = b; }
 * }
 */

public class Solution {
    /**
     * @param n: An integer
     * @param m: An integer
     * @param operators: an array of point
     * @return: an integer array
     */
    int[] bigBrothers;
    int size;
    
    private void union(int a, int b) {
        int fa = find(a, bigBrothers);
        int fb = find(b, bigBrothers);
        
        if (fa != fb) {
            bigBrothers[fa] = fb;
            size--; // union happens when searching adjacent point. Since only one point can be searched in one search loop, the size can only be decreased by 1 .
        }
    }
    
    private int find(int x, int[] f) {
        int j, fx;
        
        j = x;
        while (f[j] != j) {
            j = f[j];
        }
        
        while (x != j) {
            fx = f[x];
            f[x] = j;
            x = fx;
        }
        
        return j;
    }
    
    public List<Integer> numIslands2(int n, int m, Point[] operators) {
        // write your code here
        List<Integer> results = new ArrayList<>();
        
        if (operators == null || operators.length == 0) {
            return results;
        }
        
        // Make each point the bigBrother of itself
        bigBrothers = new int[n * m];
        boolean[][] converted = new boolean[n][m];
        
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                bigBrothers[i * n + j] = i * n + j;
                converted[i][j] = false;
            }
        }
        
        // Search each operator
        int[] dirs = {-1, 0, 1, 0, -1};
        size = 0;
        
        for (Point operator : operators) {
            int cx = operator.x;
            int cy = operator.y;
            
            // check if Point(cx, cy) is island. If it is,there is no new isolated island found. Add size to results and continue
            if (converted[cx][cy]) {
                results.add(size);
                continue;
            }
            
            // if Point(cx, cy) is not island, mark convert it and increase size by 1, because for now just deem it as an isolated island. If there are existing island connnected with it, decrease the size then.
            converted[cx][cy] = true;
            size += 1;
             
            for (int k = 0; k < 4; k++) {
                int nx = cx + dirs[k];
                int ny = cy + dirs[k + 1];
                
                // if nx and ny are within bond and Point(nx, ny) is an island, union Point(cx, cy) and Point(nx, ny)
                if (0 <= nx && nx < n && 0 <= ny && ny < m && converted[nx][ny]) {
                    union(nx * n + ny, cx * n + cy);
                }
            }
            
            // after size modification in the union process, now the size can be added to results
            results.add(size);
        }
        
        return results;
    }
}

```

# Complexity Analysis
**Time Complexity**: Initialization process will take n*m time. Searching process will take a * 4 * log(* n) time given a as the total number of operators. Therefore, the overall time complexity will be O(n * m)

**Space Complexity**: O(n * m)

# Follow Up
