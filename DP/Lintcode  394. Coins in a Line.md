# Problem
Lintcode 394. Coins in a Line

# Problem description
There are n coins in a line. Two players take turns to take one or two coins from right side until there are no more coins left. The player who take the last coin wins.

Could you please decide the first player will win or lose?

If the first player wins, return true, otherwise return false.

**Example**:
```
Input: 1
Output: true
```

```
Input: 4
Output: true
Explanation:
The first player takes 1 coin at first. Then there are 3 coins left.
Whether the second player takes 1 coin or two, then the first player can take all coin(s) left.
```

**Notice**:
```

```
# Tool kits
Dynamic programming

# Thoughts
**status**: In such a game theory type of question, the way we check whether a player will win is to check whether the other player is doomed to win. If the other player is not doomed to win, since both players are considered rational, this player is doomed to win. <br/> In this question, we can use f(n) to represent whether a player will win if there are n coins when this player takes the turn. When the game starts and the first player takes turn, there are still n coins. Therefore, f(n) means whether the first player can win. <br/><br/>

**transfer equation**:  f[i] = true if f[i - 1] == false OR f[i - 2] == false <br/><br/>


**starting status and border**: f[0] == false. If when the player takes turn there are no coins left, this play is lost. <br/><br/> 

**calculating sequence**: f[0], f[1], ... , f[n]


# Difficulties:


# Code (JAVA)
```java
public class Solution {
    /**
     * @param n: An integer
     * @return: A boolean which equals to true if the first player will win
     */
    public boolean firstWillWin(int n) {
        // status: f[n] indicates whether a player is doomed to win if there are still n coins
        boolean[] f = new boolean[3];
        
        // transfer function: if the player taking last turn is not doomed to win, then the player taking this turn is doomed to win
        for (int i = 0; i <= n; i++) {
            // border: When there is only 0 coins remaining when the player takes the turn, this player is doomed to lose
            if (i == 0) {
                f[i % 3] = false;
                continue;
            }
            
            // border: When there are 1 or 2 coins when the player takes the turn, this player is doomed to win
            if (i == 1 || i == 2) {
                f[i % 3] = true;
                continue;
            }
            
            // the current player can either take 1 coin or 2 coins
            if (f[(i - 1) % 3] == false || f[(i - 2) % 3] == false) {
                f[i % 3] = true;
            } else {
                f[i % 3] = false;
            }
        }
        
        return f[n % 3];
    }
}

```

# Complexity Analysis
**Time Complexity**: O(n) given n as the total number of coins.

**Space Complexity**: O(1) when using rolling array to optimize.

# Follow Up
