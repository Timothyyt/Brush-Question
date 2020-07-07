# Problem
Lintcode 442. Implement Trie (Prefix Tree)

# Problem description
Implement a Trie with insert, search, and startsWith methods.



**Example**:
```
Input:
  insert("lintcode")
  search("lint")
  startsWith("lint")
Output:
  false
  true

```

```
Input:
  insert("lintcode")
  search("code")
  startsWith("lint")
  startsWith("linterror")
  insert("linterror")
  search("lintcode“)
  startsWith("linterror")
Output:
  false
  true
  false
  true
  true
```

**Notice**:
```
You may assume that all inputs are consist of lowercase letters a-z.
```
# Tool kits
Trie

# Thoughts
A Trie is used in Stirng related questions. It can search whether a string is existed. HashMap can also achieve this, but it might waste a lot of space. TrieNode can save space since many words in the dictionary might share the same prefix. <br/><br/>

In order to build a Trie, TrieNode is needed. A TrieNode is a class that contains all its sons(also TrieNode) and that indicates whether the TrieNode is a word in the dictionary. In order words, TrieNode represent a prefix and one edge between two TrieNode indicates the letter that will be added to the older prefix.

# Difficulties:


# Code (JAVA)
```java
class TrieNode{
    public TrieNode[] sons;
    public boolean isWord;
    
    public TrieNode() {
        sons = new TrieNode[26]; //since input can only be lowercase letters from a-z, son must be one of these 26 letters
        for (int i = 0; i < 26; i++) {
            sons[i] = null;
        }
        
        isWord = false;
    }
}

public class Trie {
    TrieNode root;
    
    public Trie() {
        // do intialization if necessary
        root = new TrieNode();
    }

    /*
     * @param word: a word
     * @return: nothing
     */
    public void insert(String word) {
        // write your code here
        TrieNode curt = root;
        
        char[] chars = word.toCharArray();
        for (char c : chars) {
            int cIndex = c - 'a';
            
            // Create a trie that contains all the letters for one word. If one letter is not existed, build the TrieNode
            if (curt.sons[cIndex] == null) {
                curt.sons[cIndex] = new TrieNode();
            }
            
            curt = curt.sons[cIndex];
        }
        
        curt.isWord = true;
    }

    /*
     * @param word: A string
     * @return: if the word is in the trie.
     */
    public boolean search(String word) {
        // search whether each letter is the son of the current root, and keep on moving root downwards
        TrieNode curt = root;
        
        for (int i = 0; i < word.length(); i++) {
            int pos = word.charAt(i) - 'a';
            if (curt.sons[pos] == null) {
                return false;
            }
            curt = curt.sons[pos];
        }
        
        return curt.isWord;
    }

    /*
     * @param prefix: A string
     * @return: if there is any word in the trie that starts with the given prefix.
     */
    public boolean startsWith(String prefix) {
        // write your code here
        TrieNode curt = root;
        
        for (int i = 0; i < prefix.length(); i++) {
            int pos = prefix.charAt(i) - 'a';
            if (curt.sons[pos] == null) {
                return false;
            }
            curt = curt.sons[pos];
        }
        
        return true;
    }
}

```

# Complexity Analysis
**Time Complexity**: <br/> insert: The length of word inserted. <br/> search: The length of word searched <br/> startWith: The length of prefix

**Space Complexity**: Given the length of longest word in the dictionary as m, Trie is a tree with a maximum height of m. Given the number of words as w, the worst case space complexity will be O(w * m).Since words might share the same prefix, the space used in practice might be much less than the worst case scenario.

# Follow Up

