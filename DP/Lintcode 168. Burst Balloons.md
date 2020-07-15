# Problem
Lintcode 168. Burst Balloons

# Problem description
Given n balloons, indexed from 0 to n-1. Each balloon is painted with a number on it represented by array nums. You are asked to burst all the balloons. If the you burst balloon i you will get nums[left] * nums[i] * nums[right] coins. Here left and right are adjacent indices of i. After the burst, the left and right then becomes adjacent.

Find the maximum coins you can collect by bursting the balloons wisely.

**Example**:
```
Input：[4, 1, 5, 10]
Output：270
Explanation：
nums = [4, 1, 5, 10] burst 1, get coins 4 * 1 * 5 = 20
nums = [4, 5, 10]   burst 5, get coins 4 * 5 * 10 = 200 
nums = [4, 10]    burst 4, get coins 1 * 4 * 10 = 40
nums = [10]    burst 10, get coins 1 * 10 * 1 = 10
Total coins 20 + 200 + 40 + 10 = 270
```

```
Input：[3,1,5]
Output：35
Explanation：
nums = [3, 1, 5] burst 1, get coins 3 * 1 * 5 = 15
nums = [3, 5] burst 3, get coins 1 * 3 * 5 = 15
nums = [5] burst 5, get coins 1 * 5 * 1 = 5
Total coins 15 + 15 + 5  = 35
```

**Notice**:
```
You may imagine nums[-1] = nums[n] = 1. They are not real therefore you can not burst them.
0 ≤ n ≤ 500, 0 ≤ nums[i] ≤ 100
```
# Tool kits
Dynamic programming

# Thoughts
**status**: Suppose the last balloon to burst is denoted as k. Since there are two not-real balloons right next to left-end and right-end of all the balloons, use A and B to denote them respectively, the whole balloon sequence can be segmented into the following parts: 
'''
A ... k ... B
'''
The coins collected from [A, B] is coins from [A + 1, k - 1] + coins from [k + 1, B - 1] + A * k * B. This is how to get the sub question. <br/>

Use f[i][j] to denote the maximum coins that can be gotten from [i, j]. Use k to denote the last balloon to burst. k should be between i and j.  

<br/><br/>

**transfer equation**:  f[i][j] = max{f[i][k] + f[[k][j] + A[i] * A[k] * A[j]} (i < k < j). <br/><br/>

**starting status and border**: If two balloons that cannot be broken are adjacnet (two virtual balloons are next to each other), no coins can be gotten between them. f[i][i + 1] = 0. <br/><br/> 

**calculating sequence**: len = 1: f[0][1], ... , f[n][n + 1]; len = 2: f[0][2], ... , f[n - 1][n + 1]; ... ; len = n + 1, f[0][n + 1]


# Difficulties:
How to denote the two virtual balloons right outside of the left end and right end? <br/>
Solution: Modify nums[] array. Shift elements in it one step to the right. Originally the indexes are from 0 to n - 1, shift them and make them 1 to n. Then insert virtual balloon on the left end side to nums[0], and that on the right end side to nums[n + 1].


# Code (JAVA)
```java
public class Solution {
    /**
     * @param nums: A list of integer
     * @return: An integer, maximum coins
     */
    public int maxCoins(int[] AA) {
        // write your code here
        int n = AA.length;
        
        // shift balloon index from 0 ... n - 1 to 1 ... n. I do so because I wish to insert AA[-1] and AA[n], but index -1 is not compatible with normal starting index, which is 0. To make TF more conveniently represent these two virtual ballons, one step right shift is necessary.
        int[] A = new int[n + 2]; 
        // insert two border ballons to array
        A[0] = 1;
        A[n + 1] = 1;
        
        for (int i = 1; i <= n; i++) {
            A[i] = AA[i - 1];
        }
        
        // status: f[i][j] indicates if balloon i and balloon j are not burst, the maximum coin that can be gotton by bursting balloons from i + 1 to j - 1
        n += 2; // because we need to consider the two virtual ballons on the left side and right side
        int[][] f = new int[n][n]; // balloon index up to n - 1
        
        // border: if two ballons are adjacent, no ballons are between them and thus no coins can be gotten by bursting ballons between them
        for (int i = 0; i < n - 1; i++) {
            f[i][i + 1] = 0;
        }
        
        // TF: suppose one ballon k is the last one to burst, and k is between i and j, then f[i][j] will be the maximum coin gotten by bursting balloon [i, k] + maximum coin gotten from bursting ballon [k, j] + A[i] * A[k] * A[j]
        // Subquestion is to find f[i][j] within a smaller range. Therefore we need to test f[i][j] when the length between i and j get larger
        for (int len = 3; len <= n; len++) {
            int j = 0; // initialize stop position
            // i + len - 1 < n => i < n + 1 - len => i <= n - len
            for (int i = 0; i <= n - len; i++) {
                j = i + len - 1;
                
                f[i][j] = 0; // initialize
                
                for (int k = i + 1; k <= j - 1; k++) {
                    f[i][j] = Math.max(f[i][j], f[i][k] + f[k][j] + A[i] * A[k] * A[j]);
                }
            }
        }
        
        return f[0][n - 1];
    }
}
```

# Complexity Analysis
**Time Complexity**: Given the number of balloons as n, the time complexity will be O(n ^ 3)

**Space Complexity**: Given the number of balloons as n, the space complexity will be O(n ^ 2)

# Follow Up
