# Problem
Lintcode 676. Decode Ways II

# Problem description
A message containing letters from A-Z is being encoded to numbers using the following mapping way:

```
'A' -> 1
'B' -> 2
...
'Z' -> 26
```
Beyond that, now the encoded string can also contain the character *, which can be treated as one of the numbers from 1 to 9.
Given the encoded message containing digits and the character *, return the total number of ways to decode it.
Also, since the answer may be very large, you should return the output mod 10^9 + 7.

**Example**:
```
Input: "*"
Output: 9
Explanation: You can change it to "A", "B", "C", "D", "E", "F", "G", "H", "I".
```

```
Input: "1*"
Output: 18
```

**Notice**:
```
The length of the input string will fit in range [1, 10^5].
The input string will only contain the character * and digits 0 - 9.

```
# Tool kits
Dynamic programming

# Thoughts
Same as lintcode 512. The only difference is that in this questions we need to consider '*'. This will lead to more scenarios.

# Difficulties:


# Code (JAVA)
```java
public class Solution {
    /**
     * @param s: a message being encoded
     * @return: an integer
     */
    public int numDecodings(String s) {
        // write your code here
        int n = s.length();
        char[] sc = s.toCharArray();
        long MOD = 1000000007;
        
        if (n == 0) {
            return 0;
        }
        
        long[] f = new long[3];
        
        // borders
        f[0] = 1;
        f[1] = cnt1(sc[0]);
        
        for (int i = 2; i <= n; i++) {
            f[i % 3] = f[(i - 1) % 3] * cnt1(sc[i - 1]) + f[(i - 2) % 3] * cnt2(sc[i - 2], sc[i - 1]);
            
            f[i % 3] %= MOD;
        }
        
        return (int)f[n % 3];
    }
    
    private int cnt1(char c) {
        if (c == '0') {
            return 0;
        }
        
        if (c >= '1' && c <= '9') {
            return 1;
        }
        
        // c == '*'
        return 9; // * can represent 1-9
    }
    
    private int cnt2(char c1, char c2) {
        if (c1 == '0') {
            return 0;
        }
        
        if (c1 == '1') {
            if (c2 == '*') {
                return 9;
            } 
            
            // c2 == 0 - 9
            return 1;
        }
        
        if (c1 == '2') {
            if (c2 == '*') {
                return  6; // 21 - 26
            }
            
            if (c2 <= '6') {
                return 1; //20 - 26
            }
            
            return 0;
        }
        
        if (c1 >= '3' && c1 <= '9') {
            return 0;
        }
        
        if (c1 == '*') {
            if (c2 == '*') {
                return 15; //11 - 19, 21 - 26
            }
            
            if (c2 <= '6') {
                return 2; //10, 20, .... 16, 26
            }
            
            if (c2 >= '7' && c2 <= '9') {
                return 1; // 17 - 19
            }
            
            return 0;
        }
        
        return 0;
    }
}
```

# Complexity Analysis
**Time Complexity**: Given the total number of digits as n, the time complexity will be O(n)

**Space Complexity**: O(n). Even though f[] has been optimized, a charArray is created.It can be furthur optimized though, by using string.CharAt()

# Follow Up
