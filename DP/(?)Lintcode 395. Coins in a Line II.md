# Problem
Lintcode 395. Coins in a Line II

# Problem description
There are n coins with different value in a line. Two players take turns to take one or two coins from left side until there are no more coins left. The player who take the coins with the most value wins.

Could you please decide the first player will win or lose?

If the first player wins, return true, otherwise return false.


**Example**:
```
Input: [1, 2, 2]
Output: true
Explanation: The first player takes 2 coins.
```

```
Input: [1, 2, 4]
Output: false
Explanation: Whether the first player takes 1 coin or 2, the second player will gain more value.
```

**Notice**:
```

```
# Tool kits
Dynamic programming

# Thoughts
**status**: Suppose the sum of values of playerA's coins is A, that of playerB's is B. playerA's targer is to make sure A>=B, while playerB's target is to make sure B>=A. Use Sa to denote A - B, and Sb to denote B - A. Suppose playerA is the first one to pick coins, and he picks coins with a total value of m. Then playerA will wish that m + A' - B' >= 0 (A' means the sum of values of remaining coins playerA can get, and B' means that of playerB). Therefore, Sa = m - (B' - A') = m - Sb. Then when there are no coins, we can check whether Sa >= 0 to decide whether the first player can win<br/><br/>

**transfer equation**: f[i] = max{values[i] - f[i + 1], values[i] + values[i + 1] - f[i + 2]} <br/><br/>

**starting status and border**: f[n] = 0, because when the game just starts, neither of players have picked coins. Therefore, there is no difference in the value of coins they hold.<br/><br/>

**calculating sequence**: f[n], f[n - 1], ..., f[0]


# Difficulties:


# Code (JAVA)
```java
public class Solution {
    /**
     * @param values: a vector of integers
     * @return: a boolean which equals to true if the first player will win
     */
    public boolean firstWillWin(int[] values) {
        // status: f[i] means the biggest advantage one player can gain over another when there are still i coins.
        int n = values.length;
        int[] f = new int[n + 1];
        
        for (int i = n; i >=0; i--) {
            // border: at the beginning of the game, neither of two players have ever picked coins. Therefore, there is no difference in their coin values
            if (i == n) {
                f[i] = 0;
                continue;
            }
            
            if (i == n - 1) {
                f[i] = values[i] - f[i + 1];
                continue;
            }
            
            // TF: 
            f[i] = Math.max(values[i] - f[i + 1], values[i] + values[i + 1] - f[i + 2]);
        }
        
        return f[0] > 0;
    }
}
```

# Complexity Analysis
**Time Complexity**: O(n) given n as number of coins

**Space Complexity**: O(n)

# Follow Up
