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
**status**: See code comment.

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
        // status: f[i] means when there are i coins, the biggest advantage the player who takes this turn can gain over the other player.
        int n = values.length;
        
        int[] f = new int[3];
        
        // border: When the game just starts, there are still n coins left, there is no difference in player's score
        f[n % 3] = 0;
        
        // border: when only one coin is taken, f[n] = values[n - 1] - f[n - 1] => f[n - 1] = values[n - 1] - f[n]
        f[(n - 1) % 3] = values[n - 1] - f[n % 3];
        
        // TF: If the player who takes this turn picks one coin: values[i], then the biggest advantage the other player has against this player when it is the other player's turn is f[i - 1]. Therefore, the biggest advantage this player can have over the other player is values[i] - f[i - 1]. If this player takes two coins, then his advantage over the other player will be values[i] + values[i - 1] - f[i - 2].
        // However, the result we wish to have is f[0]. If we use TF above, we will have f[-1], f[-2]... , which does not exist. We have to modify the equation from f[i] = values[i] - f[i - 1] to f[i - 1] = values[i] - f[i], and f[i] = values[i] + values[i - 1] - f[i - 2] to f[i] = values[i + 1} + values[i] -f[i + 2]
        for (int i = n - 2; i >= 0; i--) {
            f[i % 3] = Math.max(values[i] - f[(i + 1) % 3], values[i] + values[i + 1] - f[(i + 2) % 3]);
        }
        
        return f[0 % 3] >= 0;
    }
}
```

# Complexity Analysis
**Time Complexity**: O(n) given n as number of coins

**Space Complexity**: O(1) after using rolling array

# Follow Up
