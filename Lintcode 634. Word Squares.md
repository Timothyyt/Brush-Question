# Problem
 Lintcode 634. Word Squares

# Problem description
Given a set of words without duplicates, find all word squares you can build from them.

A sequence of words forms a valid word square if the kth row and column read the exact same string, where 0 ≤ k < max(numRows, numColumns).

For example, the word sequence ["ball","area","lead","lady"] forms a word square because each word reads the same both horizontally and vertically.

<br/>b a l l
<br/>a r e a
<br/>l e a d
<br/>l a d y



**Example**:
```
Input:
["area","lead","wall","lady","ball"]
Output:
[["wall","area","lead","lady"],["ball","area","lead","lady"]]

Explanation:
The output consists of two word squares. The order of output does not matter (just the order of words in each word square matters).
```

```
Input:
["abat","baba","atan","atal"]
Output:
 [["baba","abat","baba","atan"],["baba","abat","baba","atal"]]
```

**Notice**:
```
There are at least 1 and at most 1000 words.
All words will have the exact same length.
Word length is at least 1 and at most 5.
Each word contains only lowercase English alphabet a-z.
```
# Tool kits
trie

# Thoughts
The brute-force method is to try each combination of words and check whether they can form a square. If 1000 words are given, then we need to try 1000 ^ 1000 times since words can be reused and for each row of the 1000 rows there are 1000 options. This is too slow. <br/><br/> In order to optimize the process, we can use trie to prune. Just take the word sequence in the problem description as an example. when we start searching with word 'ball', the other three rows will have to start with 'a','l','l' respectively. Then we add area to square since area starts with 'a'. Then the other two rows need to start with le and la respectively. In this way we can find the required prefixes. Then we search these prefixes on the trie to see if it can be found. If one required prefix cannot be found, we don't need to keep on searching. The pruning can be achieved in this way. <br/><br/> As for adding new words to the square, we can try the words with the required preifx. This is another way to optimize the process.


# Difficulties:
1. Don't forget deep copy in recursion! <br/><br/>
2. Check code to see how to correctly get the required prefix


# Code (JAVA)
```java
class TrieNode{
    TrieNode[] sons;
    boolean isWord;
    ArrayList<String> wordList; // stores the words that has this prefix
    
    public TrieNode() {
        sons = new TrieNode[26]; // a-z
        isWord = false;
        wordList = new ArrayList<String>();
    }
}


class Trie{
    TrieNode root = new TrieNode();
    
    public void insert(String word) {
        TrieNode curt = root;
        
        for (int i = 0; i < word.length(); i++) {
            int c = word.charAt(i) - 'a';
            if (curt.sons[c] == null) {
                curt.sons[c] = new TrieNode();
            }
            
            curt = curt.sons[c];
            curt.wordList.add(word);
        }
        
        curt.isWord = true;
    } 
    
    public TrieNode find(String word) {
        TrieNode curt = root;
        
        for (int i = 0; i < word.length(); i++) {
            int c = word.charAt(i) - 'a';
            
            if (curt.sons[c] == null) {
                return null;
            }
            
            curt = curt.sons[c];
        }
        
        return curt;
    }
    
    public ArrayList<String> getWordsWithPrefix(String prefix) {
        TrieNode node = find(prefix);
        
        if (node == null) {
            return new ArrayList<>();
        }
        
        return node.wordList;
    }
}

public class Solution {
    /*
     * @param words: a set of words without duplicates
     * @return: all word squares
     */
    public List<List<String>> wordSquares(String[] words) {
        
        List<List<String>> results = new ArrayList<>();
        
        // corner case
        if (words == null || words.length == 0 || words[0].length() == 0) {
            return results;
        }
        
        // insert all the words to trie
        Trie trie = new Trie();
        for (String word : words) {
            trie.insert(word);
        }
        
        // pick one word and start searching from this word
        for (String word : words) {
            List<String> square = new ArrayList<>();
            square.add(word);
            search(trie, square, results);
        }
        
        return results;
    }
    
    private void search(Trie trie,
                        List<String> square,
                        List<List<String>> results)
    {
        // recusion exit: the result has a shape of square
        int wordNum = square.size();
        int wordLen = square.get(0).length();
        
        //System.out.println(wordNum);
        //System.out.println(wordLen);
        
        if (wordNum == wordLen) {
            results.add(new ArrayList<String>(square));
            return;
        }
        
        // pruning
        for (int i = wordNum; i < wordLen; i++) {
            String prefix = "";
            for (int j = 0; j < wordNum; j++) {
                prefix = prefix + square.get(j).charAt(i);
                //System.out.println(prefix);
                
            // search this prefix on the trie. If not found, it is impossible to form a square.
                if (trie.find(prefix) == null) {
                    return;
                }
            }
        }
        
        // get the prefix for new rows and add word with this prefix to square and then search.
        // the second row will start with ball[0][1] which is 'a'
        // the third row will start with ball[0][2] + ball[1][2] which is 'l' + 'e' = 'le'
        String nextRowPrefix = "";
        for (int i = 0; i < wordNum; i++) {
            nextRowPrefix = nextRowPrefix + square.get(i).charAt(wordNum);
        }
        
        for (String word : trie.getWordsWithPrefix(nextRowPrefix)) {
            square.add(word);
            System.out.println(square);
            search(trie, square, results);
            square.remove(word);
        }
    }
}

```

# Complexity Analysis
**Time Complexity**: Cannot be accurately analyzed, but should be much better than the brute force method.

**Space Complexity**: 

# Follow Up
