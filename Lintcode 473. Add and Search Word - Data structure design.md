# Problem
Lintcode 473. Add and Search Word - Data structure design

# Problem description
Design a data structure that supports the following two operations: addWord(word) and search(word)

search(word) can search a literal word or a regular expression string containing only letters a-z or ..

A . means it can represent any one letter.



**Example**:
```
Input:
  addWord("a")
  search(".")
Output:
  true

```

```
Input:
  addWord("bad")
  addWord("dad")
  addWord("mad")
  search("pad")  
  search("bad")  
  search(".ad")  
  search("b..")  
Output:
  false
  true
  true
  true
```

**Notice**:
```
You may assume that all words are consist of lowercase letters a-z.
```
# Tool kits
Trie

# Thoughts
This questions is similar to trie implementation. The only difficulty is to deal with '.' properly. Whenever a '.' is searched, since it can represent every lowercase letter from a-z, all the options need to be tested to see if a word can be found. This can be achieved recursively. Write a helper function for more convenient recursion. This helper function should have three input: 1. the word being searched 2. the position of the letter being searched in the word 3. position of node in the Trie.


# Difficulties:


# Code (JAVA)
```java
class TrieNode{
    TrieNode[] sons;
    boolean isWord;
    
    public TrieNode() {
        sons = new TrieNode[26];
        for (int i = 0; i < 26; i++) {
            sons[i] = null;
        }
        
        isWord = false;
    }
}

public class WordDictionary {
    /*
     * @param word: Adds a word into the data structure.
     * @return: nothing
     */
    TrieNode root = new TrieNode();
    
    public void addWord(String word) {
        // write your code here
        TrieNode curt = root;
        
        for (int i = 0; i < word.length(); i++) {
            int c = word.charAt(i) - 'a';
            if (curt.sons[c] == null) {
                curt.sons[c] = new TrieNode();
            }
            
            curt = curt.sons[c];
        }
        
        curt.isWord = true;
    }

    /*
     * @param word: A word could contain the dot character '.' to represent any one letter.
     * @return: if the word is in the data structure.
     */
    public boolean search(String word) {
        // write your code here
        return searchHelper(word, 0, root);
    }
    
    // search both on the Trie and in the words
    public boolean searchHelper(String word, int index, TrieNode node) {
        // if already searching the end of word, see if it isWord
        if (index == word.length()) {
            return node.isWord;
        }
        
        // if the element being searched is '.', 
        // try to use all the letters from a-z to replace '.' and see if the word can be searched.
        if (word.charAt(index) == '.') {
            for (int i = 0; i < 26; i++) {
                // cannot directly return searchHelper here because for the 26 situations, as long as one is true, the result is true. Have to see all of results.
                if (node.sons[i] != null && searchHelper(word, index + 1, node.sons[i])) {
                    return true;
                }
            }
            
            // if word is not searched under any situation
            return false;
        }
        
        // if the element being searched is letter a-z
        int c = word.charAt(index) - 'a';
        
        if (node.sons[c] == null) {
            return false;
        }
        
        return searchHelper(word, index + 1, node.sons[c]);
    }
}

```

# Complexity Analysis
**Time Complexity**: <br/> addWord: given the length of word added as m, the time complexity of addWord is O(m) <br/> search: given the length of word searched is m, the worst case scenario time complexity will be 26 ** m. This will happen when only using '.' to search, say search('...') <br/><br/>
**Space Complexity**: Given the total number of letters of the words added is n, the space complexity will be O(n).
