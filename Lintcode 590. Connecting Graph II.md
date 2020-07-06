# Problem
Lintcode 590. Connecting Graph II

# Problem description

Given n nodes in a graph labeled from 1 to n. There is no edges in the graph at beginning.

You need to support the following method:

connect(a, b), an edge to connect node a and node b
query(a), Returns the number of connected component nodes which include node a.


**Example**:
```
Input:
ConnectingGraph2(5)
query(1)
connect(1, 2)
query(1)
connect(2, 4)
query(1)
connect(1, 4)
query(1)
Output:[1,2,3,3]
```

```
Input:
ConnectingGraph2(6)
query(1)
query(2)
query(1)
query(5)
query(1)

Output:
[1,1,1,1,1]
```

**Notice**:
```

```
# Tool kits
Union-find

# Thoughts
Similar to lintcode 589, it is still a basic implementation of union find.

# Difficulties:
1. How to record the connected nodes for each node? <br/><br/>
Solution: Record the size of the bigBrother and return this size as the number of connected nodes.

# Code (JAVA)
```java
public class ConnectingGraph2 {
    /*
    * @param n: An integer
    */
    private int[] bigBrothers;
    private int[] sizes; // record the size of connected nodes of each node
    
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
    
    
    public ConnectingGraph2(int n) {
        // do intialization if necessary
        bigBrothers = new int[n + 1];
        sizes = new int[n + 1];
        
        for (int i = 1; i <= n; i++) {
            bigBrothers[i] = i;
            sizes[i] = 1;
        }
    }

    /*
     * @param a: An integer
     * @param b: An integer
     * @return: nothing
     */
    public void connect(int a, int b) {
        // write your code here
        int fa = find(a, bigBrothers);
        int fb = find(b, bigBrothers);
        
        if (fa != fb) {
            sizes[fb] += sizes[fa];
        }
        
        bigBrothers[fa] = fb;
    }

    /*
     * @param a: An integer
     * @return: An integer
     */
    public int query(int a) {
        // write your code here
        int fa = find(a, bigBrothers);
        return sizes[fa];
    }
}
```

# Complexity Analysis
**Time Complexity**: 
<br/> connect: O(log *n) ==> O(1) <br/><br/> query: O(1)

**Space Complexity**: <br/> O(n)

