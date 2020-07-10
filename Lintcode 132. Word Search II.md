# Problem
Lintcode 132. Word Search II

# Problem description
Given a matrix of lower alphabets and a dictionary. Find all words in the dictionary that can be found in the matrix. A word can start from any position in the matrix and go left/right/up/down to the adjacent position. One character only be used once in one word. No same word in dictionary



**Example**:
```
Input：["doaf","agai","dcan"]，["dog","dad","dgdg","can","again"]
Output：["again","can","dad","dog"]
Explanation：
  d o a f
  a g a i
  d c a n
search in Matrix，so return ["again","can","dad","dog"].
```

```
Input：["a"]，["b"]
Output：[]
Explanation：
 a
search in Matrix，return [].
```

**Notice**:
```

```
# Tool kits
trie, dfs

# Thoughts
A brute force solution is to pick a starting point from dictionary, and keep on searching the whole board to find words. The board will thus be searched n * m times and in the worst case, where one word consist of all the chars in the board, 4 * n * m time is needed for the searching. <br/><br/> One way to prune the whole process is to use trie. Firstly insert all the words to trie. Then search both in dictionary and on the trie. If no words can be found given the prefix on trie, then there is no need to search this route. 

# Difficulties:


# Code (JAVA)
```java
class TrieNode{
    Map<Character, TrieNode> sons;
    String word;
    
    public TrieNode() {
        sons = new HashMap<>();
        word = null;
    }
}

class Trie{
    TrieNode root;
    
    public Trie() {
        root = new TrieNode();
    }
    
    public void insert(String word) {
        TrieNode curt = root;
        for (int i = 0; i < word.length(); i++) {
            char c = word.charAt(i);
            if (curt.sons.get(c) == null) {
                curt.sons.put(c, new TrieNode());
            }
            
            curt = curt.sons.get(c);
        }
        
        curt.word = word; // prefix till current node will be a word
    }
}


public class Solution {
    /**
     * @param board: A list of lists of character
     * @param words: A list of string
     * @return: A list of string
     */
    public List<String> wordSearchII(char[][] board, List<String> words) {
        // write your code here
        List<String> results = new ArrayList<>();
        
        // corner case
        if (board == null || board.length == 0 || board[0].length == 0) {
            return results;
        }
        
        // insert all the words to trie
        Trie trie = new Trie();
        
        for (String word : words) {
            trie.insert(word);
        }
        
        // locate starter in dictionary and search in dictionary to see whether a word in words can be found
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[i].length; j++) {
                search(board, i, j, trie.root, results);
            }
        }
        
        return results;
    }
    
    private int[] dirs = {-1, 0, 1, 0, -1};
    
    private void search(char[][] board, int cx, int cy, TrieNode node, List<String> results) {
        
        // exit of recursion: if board[cx][cy] is not a prefix of any given word, there is no need to search this point.
        if (!node.sons.containsKey(board[cx][cy])) {
            return;
        }
        
        // See if a word has been found, if yes, add to results.
        TrieNode son = node.sons.get(board[cx][cy]);
        if (son.word != null) {
            if (!results.contains(son.word)) {
                results.add(son.word);
            }
        }
        
        // Keep on finding if there are other words with this prefix
        
        // record board[cx][cy] as visited
        char tmp = board[cx][cy];
        board[cx][cy] = 0;
        
        for (int k = 0; k < 4; k++) {
            int nx = cx + dirs[k];
            int ny = cy + dirs[k + 1];
            
            if (0 <= nx && nx < board.length && 0 <= ny && ny < board[0].length && board[nx][ny] != 0) {
                search(board, nx, ny, son, results);
            }
        }
        
        board[cx][cy] = tmp; // backtracking
    }
}
```

# Complexity Analysis
**Time Complexity**: Given the sum of letters in all the words as k, trie insert will take O(k) time. With the aid of trie, only letter combinations of words will be searched in the dictionary. This will also take O(k) time. Given the length and width of board as n and m respectively, locating the starting time will take O(n * m) time. Therefore, the overall time complexity is O(n * m * k).

**Space Complexity**:  Given the sum of letters in all the words as k, the space complexity will be O(k), which is the space used by trie.

# Follow Up
