# Problem
512. Decode Ways

# Problem description
A message containing letters from A-Z is being encoded to numbers using the following mapping:

```
'A' -> 1
'B' -> 2
...
'Z' -> 26
```

Given an encoded message containing digits, determine the total number of ways to decode it.

**Example**:
```
Input: "12"
Output: 2
Explanation: It could be decoded as AB (1 2) or L (12).
```

```
Input: "10"
Output: 1
```

**Notice**:
```
we can't decode an empty string. So you should return 0 if the message is empty.
The length of message n \leq 100n≤100
```
# Tool kits
Dynamic programming

# Thoughts
**status**: Use f[i] to represent the total number of ways to denode the first i digits. Since the last letter might take one digit (eg. 'A') or two digits (eg.'Z'), given the number of digits as n, we need to know f[n - 1] and f[n - 2] in order to get f[n].  <br/><br/>

**transfer equation**:  f[n] = f[n - 1] * cnt1 + f[n - 2] * cnt2. cnt1 means the number of ways to decode one digit, ad cnt2 means the way to decode two digits. For cnt1, as long as the digits is within range 1-9, there is 1 way to decode it. For cnt2, when the ten digit is 1, there will to 1 way to decode if one digit number if 0 - 9. When the ten digit number is 2, there will be a way to denode only when the one digit is 0 - 6. All other ten digit number will lead to 0 decode method. <br/><br/>

**starting status and border**: f[0] = 1 because when the string is empty we can only return 0. f[1] = cnt1. <br/><br/> 

**calculating sequence**: f[0], f[1], f[2], ... , f[n]


# Difficulties:


# Code (JAVA): get cnt by analyzing char
```java
public class Solution {
    /**
     * @param s: a string,  encoded message
     * @return: an integer, the number of ways decoding
     */
    public int numDecodings(String s) {
        // write your code here
        char[] sc = s.toCharArray();
        int n = s.length();
        
        if (n == 0) {
            return 0;
        }
        
        // status: f[i] indicatest the total number of ways to decode the first i letters.
        int[] f = new int[3];
        
        for (int i = 0; i <= n; i++) {
            if (i == 0) {
                f[i % 3] = 1; // there is only one way to decode an empty string, which is to return 0
                continue;
            }
            
            f[i % 3] = f[(i - 1) % 3] * cnt1(sc[i - 1]);
            
            if (i > 1) {
                f[i % 3] += f[(i - 2) % 3] * cnt2(sc[i - 2], sc[i - 1]);
            }
        }
        
        return f[n % 3];
    }
    
    private int cnt1(char c) {
        if (c >= '1' && c <= '9') {
            return 1;
        }
        
        return 0;
    }
    
    private int cnt2(char c1, char c2) {
        
        if (c1 == '1') {
            return 1; // 10 - 19 all qualify
        }
        
        if (c1 == '2') {
            if (c2 >= '0' && c2 <= '6') {
                return 1;
            } else {
                return 0;
            }
        }
        
        // if c1 == 0 or c1 >= 3
        return 0;
    }
}

```

# Code (JAVA): get cnt by analyzing subString
```java
public class Solution {
    /**
     * @param s: a string,  encoded message
     * @return: an integer, the number of ways decoding
     */
    public int numDecodings(String s) {
        // corner case: empty string
        if (s == null || s.length() == 0) {
            return 0;
        }
        
        int n = s.length();
        int[] f = new int[3];
        
        f[0] = 1;
        f[1] = decoder(s.substring(0, 1));
        
        for (int i = 2; i <= n; i++) {
            f[i % 3] = f[(i - 1) % 3] * decoder(s.substring(i - 1, i)) + f[(i - 2) % 3] * decoder(s.substring(i - 2, i));
        }
        
        return f[n % 3];
    }
    
    private int decoder(String subStr) {
        int code = Integer.parseInt(subStr);
        
        if (subStr.length() == 1 && 1 <= code && code <= 9) {
            return 1;
        }
        
        if (subStr.length() == 2 && 10 <= code && code <= 26) {
            return 1;
        }
        
        // not valid
        return 0;
    }
}

```


# Complexity Analysis
**Time Complexity**: given number of digits as n, the tiem complexity is O(n)

**Space Complexity**: After using rolling array, the space compelxity can be optiized to O(1)

# Follow Up

