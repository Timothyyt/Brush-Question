# Problem
Lintcode 396. Coins in a Line III.md

# Problem description
There are n coins in a line, and value of i-th coin is values[i].

Two players take turns to take a coin from one of the ends of the line until there are no more coins left. The player with the larger amount of money wins.

Could you please decide the first player will win or lose?



**Example**:
```
Input: [3, 2, 2]
Output: true
Explanation: The first player takes 3 at first. Then they both take 2.
```

```
Input: [1, 20, 4]
Output: false
Explanation: The second player will take 20 whether the first player take 1 or 4.
```

**Notice**:
```

```
# Tool kits
Dynamic Programming

# Thoughts
**status**: Use f[i][j] to represent the biggest advantage one player will have over the other player. i indicates the left end index while j indicates the right end index. The remaining coins thus are always from i to j. Therefore, a range is appropriate for the status <br/><br/>

**transfer equation**:  f[i][j] = max{values[i] - f[i + 1][j], values[j] - f[i][j - 1]} <br/> When one player pick left end coin, which is values[i], the biggest advantage this player can have over the other player is values[i] - f[i + 1][j]. f[i + 1][j] is the biggest advantage the other player will get when he takes next turn. Same for the right end coin. <br/><br/>

**starting status and border**: f[i][i] = values. If left end is right end, which means there is only one coin, the player who pick this coin will get an advantage of this coin's value. <br/><br/>

**calculating sequence**: len = 1: f[0][0], ... , f[n - 1][n - 1]; len = 2: f[0][1], ... , f[n - 2][n - 1]; ... ; len = n - 1: f[0][n - 1]


# Difficulties:


# Code (JAVA)
```java
public class Solution {
    /**
     * @param values: a vector of integers
     * @return: a boolean which equals to true if the first player will win
     */
    public boolean firstWillWin(int[] values) {
        // status: f[i][j] means when the player can choose still choose coins from i to j, the biggest advantage he can have over his opponent.
        int n = values.length;
        
        int[][] f = new int[n][n];
        
        // border
        for (int i = 0; i < n; i++) {
            f[i][i] = values[i];
        }
        
        // TF: if one player has taken the coin on the left end, which is values[i], the advantage he will have over the other player is how much values[i] is higher than the biggest advantage the other player can get in the remaining [i + 1, j] coins. Same for right end. Then find the bigger on between these two situations.
        for (int len = 2; len <= n; len++) {
            // i + len - 1 < n => i < n + 1 - len => i <= n - len
            int j = 0; // initialize right end
            for (int i = 0; i <= n - len; i++) {
                j = i + len - 1;
                
                f[i][j] = Math.max(values[i] - f[i + 1][j], values[j]- f[i][j - 1]);
            }
        }
        
        return f[0][n - 1] >= 0;
    }
}
```

# Complexity Analysis
**Time Complexity**: O(n) given length of values as n. 

**Space Complexity**: O(n)

# Follow Up

