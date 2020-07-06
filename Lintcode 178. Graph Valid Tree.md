# Problem
Lintcode 178. Graph Valid Tree


# Problem description
Given n nodes labeled from 0 to n - 1 and a list of undirected edges (each edge is a pair of nodes), write a function to check whether these edges make up a valid tree.

**Example**:
```
Input: n = 5 edges = [[0, 1], [0, 2], [0, 3], [1, 4]]
Output: true.

```

```
Input: n = 5 edges = [[0, 1], [1, 2], [2, 3], [1, 3], [1, 4]]
Output: false.
```

**Notice**:
```
You can assume that no duplicate edges will appear in edges. Since all edges are undirected, [0, 1] is the same as [1, 0] and thus will not appear together in edges.
```
# Tool kits
Union-find

# Thoughts
In order to build a valid tree, there are two requirements: <br/><br/>1. The number of edges must be exactly one less than the number nodes. <br/> 2. The number of connected block must be exactly one. <br/><br/> This is a dynamic connected block question and thus union-find is suitable for dealing with it. The first requirement is easy to test. As for the second requirement, there are two ways to test it: <br/><br/> 1. Get the number of connected block and test whether it is 1. <br/> 2. find the bigBrothers for the nodes on the same edge and see if these two nodes have the same bigBrother. If the answer is yes, then there mush be more than one connected block, because there is a circle and the number of edges will thus be inadequte to form a valid tree.

# Difficulties:
1. Find our the requirements for a valid tree.


# Code-method1 (JAVA)
```java
public class Solution {
    /**
     * @param n: An integer
     * @param edges: a list of undirected edges
     * @return: true if it's a valid tree, or false
     */
    int count;
    int[] bigBrothers;
    
    private int find(int x, int[] f) {
        int j, fx;
        
        j = x;
        while (j != f[j]) {
            j = f[j];
        }
        
        while (x != j) {
            fx = f[x];
            f[x] = j;
            x = fx;
        }
        
        return j;
    }
    
    private void union(int a, int b) {
        int fa = find(a, bigBrothers);
        int fb = find(b, bigBrothers);
        
        if (fa != fb) {
            bigBrothers[fa] = fb;
            count--;
        }
    }
     
    
    public boolean validTree(int n, int[][] edges) {
        // corner case: if there is only one node, no edge is needed and it will be the tree itself
        if (n <= 1) {
            return true;
        }
        
        // corner case: if there are more than 1 node but no edge is provided, it cannot be the tree
        if (edges == null || edges.length == 0 || edges[0].length == 0) {
            return false;
        }
        
        // initialize bigBrothers
        bigBrothers = new int[n + 1];
        for (int i = 0; i < n; i++) {
            bigBrothers[i] = i;
        }
        
        
        // check if number of edges is n - 1. If not, this is absolutely not a valid tree
        int edgeCount = edges.length;
        if (edgeCount != n - 1) {
            return false;
        }
        
        // passed number of edges test
        // initialize the number of isloated blocks. Deem each node as isolated at the beginning
        count = n;

        // union the numbers in each edge
        for (int[] edge : edges) {
            union(edge[0], edge[1]);
        }
        
        // Check if there is only one block. If yes, this is a valid tree
        return count == 1;
    }
}
```

# Code-method2 (JAVA)
```java
public class Solution {
    /**
     * @param n: An integer
     * @param edges: a list of undirected edges
     * @return: true if it's a valid tree, or false
     */
    int[] father = null;
    
    private int find(int x) {
        int root = x;
        int temp;
        
        while (father[root] != root) {
            root = father[root];
        }
        
        while (x != root) {
            temp = father[x];
            father[x] = root;
            x = temp;
        }
        
        return root;
    }
    
    private void union(int a, int b) {
        int rootA = find(a);
        int rootB = find(b);
        
        if (rootA != rootB) {
            father[rootA] = rootB;
        }

    }

    
    public boolean validTree(int n, int[][] edges) {
        // two conditions of a valid tree
        // 1. number of edges is n - 1
        // 2. there is only one connected block
        // Use unionfind and add all the edges
        
        // see if condition of edges == n - 1 is met
        if (edges.length != n - 1) {
            return false;
        }
        
        
        // see if it can be a connnected block
        father = new int[n];
        
        for (int i = 0; i < n; i++) {
            father[i] = i;
        }
        
        for (int[] edge : edges) {
            // cannot be a circle
            if (find(edge[0]) == find(edge[1])) {
                return false;
            }
            
            // union two points in this edge
            union(edge[0], edge[1]);
        }
        
        return true;
    }
}
```

# Complexity Analysis
**Time Complexity**: O(n)

**Space Complexity**: O(n)

# Follow Up
