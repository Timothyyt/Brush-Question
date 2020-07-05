# Problem


# Problem description
Given n nodes in a graph labeled from 1 to n. There is no edges in the graph at beginning.

You need to support the following method:

connect(a, b), add an edge to connect node a and node b`.
query(a, b), check if two nodes are connected



**Example**:
```
Input:
ConnectingGraph(5)
query(1, 2)
connect(1, 2)
query(1, 3) 
connect(2, 4)
query(1, 4) 
Output:
[false,false,true]
```

```
Input:
ConnectingGraph(6)
query(1, 2)
query(2, 3)
query(1, 3)
query(5, 6)
query(1, 4)

Output:
[false,false,false,false,false]

```

**Notice**:
```

```
# Toolkits
Union find

# Thoughts
This question is in essence an implementation of union find. Just deem the connected elements as nodes in a tree. The node in the tree that has no father can be used to represent all the connected elements in the tree. Just call such node 'bigBrother'

The most elegent part of union-find algorithm is the path compression part. Imagine there is a tree consists of n connected elements. Each node on the tree only has one child. When path compression is not used, if we search the father of each node until the bigBrother of tree is found, then for the node at the botton, O(n) time complexity is needed for this find process. <br/><br/> By using path compression, big brother will be the father of each node on the tree except for itself. In this way, the process of finding bigBrother can be compressed to O(1).

# Difficulties:


# Code (JAVA)
```java

public class ConnectingGraph {
    
    public int find(int x, int[] f) {
        int j, fx;
    
        // find the big brother
        j = x;
        while (f[j] != j) {
            j = f[j]; 
        }
    
        // path compression
        // j is now the big brother
        while (x != j) {
            fx = f[x]; // record the father of x
            f[x] = j; // connect x with the big brother
            x = fx; // move x upwards and continue to find
        }
    
        return j; // return the big brother
    }
    
    public void union(int x, int y, int[] f) {
        int fx = find(x, f);
        int fy = find(y, f);
        f[fx] = fy; // Make one big brother the big brother of another big brother
    }
    
    
    private int[] bigBrothers;
    /*
    * @param n: An integer
    */

    public ConnectingGraph(int n) {
        // do intialization if necessary
        bigBrothers = new int[n + 1];
        for (int i = 0; i < n; i++) {
            bigBrothers[i] = i; // at the beginning, each node is isolated.
        }
    }

    /*
     * @param a: An integer
     * @param b: An integer
     * @return: nothing
     */
    public void connect(int a, int b) {
        // write your code here
        union(a, b, bigBrothers);
    }

    /*
     * @param a: An integer
     * @param b: An integer
     * @return: A boolean
     */
    public boolean query(int a, int b) {
        // write your code here
        int fa = find(a, bigBrothers);
        int fb = find(b, bigBrothers);
        
        return fa == fb;
    }
}

```

# Complexity Analysis
**Time Complexity**: <br/> Union: O(1) <br/> find: O(1) <br/> https://en.wikipedia.org/wiki/Iterated_logarithm
