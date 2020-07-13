# Problem
Lintcode 629. Minimum Spanning Tree

# Problem description
Given a list of Connections, which is the Connection class (the city name at both ends of the edge and a cost between them), find edges that can connect all the cities and spend the least amount.
Return the connects if can connect all the cities, otherwise return empty list.



**Example**:
```
Input:
["Acity","Bcity",1]
["Acity","Ccity",2]
["Bcity","Ccity",3]
Output:
["Acity","Bcity",1]
["Acity","Ccity",2]

```

```
Input:
["Acity","Bcity",2]
["Bcity","Dcity",5]
["Acity","Dcity",4]
["Ccity","Ecity",1]
Output:
[]

Explanation:
No way
```

**Notice**:
```
Return the connections sorted by the cost, or sorted city1 name if their cost is same, or sorted city2 if their city1 name is also same

```
# Tool kits
union-find

# Thoughts
Firstly sort the connections by cost in order to firstly union the cities with the least cost. Then use union-find to decide whether the two cities in one connection are in a same connected block. If the answer is yes, there is no need to add this edge to results again. If not, union these two cities and add this connection to results. Finally decide whether all the cities are properly connected by checking if the number of cities is exactly one more than the number of connections. If this requirement is met, then there will be no circle.

# Difficulties:


# Code (JAVA)
```java
/**
 * Definition for a Connection.
 * public class Connection {
 *   public String city1, city2;
 *   public int cost;
 *   public Connection(String city1, String city2, int cost) {
 *       this.city1 = city1;
 *       this.city2 = city2;
 *       this.cost = cost;
 *   }
 * }
 */


public class Solution {
    /**
     * @param connections given a list of connections include two cities and cost
     * @return a list of connections from results
     */
    private Map<String, String> bigBrothers;
    
    private String find(String x) {
        String fx, root;
        
        // get root
        root = x;
        while (!bigBrothers.get(root).equals(root)) {
            root = bigBrothers.get(root);
        }
        
        // path compression
        while (!x.equals(root)) {
            fx = bigBrothers.get(x);
            bigBrothers.put(x, root);
            x = fx;
        }
        
        return root;
    }
    
    public List<Connection> lowestCost(List<Connection> connections) {
        List<Connection> results = new ArrayList<>();
        
        // conner case: empty list
        if (connections == null || connections.size() == 0) {
            return results;
        }
        
        // initialize bigBrothers
        bigBrothers = new HashMap<String, String>();
        for (Connection connection : connections) {
            bigBrothers.put(connection.city1, connection.city1);
            bigBrothers.put(connection.city2, connection.city2);
        }
        
        // sort connections 
        Collections.sort(connections, new Comparator<Connection>(){
            public int compare(Connection a, Connection b) {
                if (a.cost != b.cost) {
                    return a.cost - b.cost;
                }
                
                if (!a.city1.equals(b.city1)) {
                    return a.city1.compareTo(b.city1);
                }
                
                return a.city2.compareTo(b.city2);
            }
        });
        
        // traverse connections and union cities that has not been unioned
        for (Connection connection : connections) {
            // find the bigBrother for both cities in this connection
            String root1 = find(connection.city1);
            String root2 = find(connection.city2);
            
            if (!root1.equals(root2)) {
                bigBrothers.put(root1, root2);
                results.add(connection);
            }
        }
        
        // if city connections are valid, return results, otherwise return an empty list
        if (bigBrothers.size() == results.size() + 1) {
            return results;
        }
        
        return new ArrayList<Connection>();
        
    }
}
```

# Complexity Analysis
**Time Complexity**: Given the number of connections as n, 'initialize bigBrothers' takes O(n), 'sort connections' takes O(nlongn), 'traverse connections and union cities that has not been unioned' takes O(n), therefore the overall time complexity is O(nlongn)

**Space Complexity**: Given the number of connections as n, the space complexity will be O(n) since a hashmap is used to store cities in each connection.

# Follow Up
