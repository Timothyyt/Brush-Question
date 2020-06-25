# Problem
https://www.lintcode.com/problem/decode-string/my-submissions

# Problem description

Given an expression s contains numbers, letters and brackets. Number represents the number of repetitions inside the brackets(can be a string or another expression)．Please expand expression to be a string.


**Example**:
```
Input: S = abc3[a]
Output: "abcaaa"
```

```
Input: S = 3[2[ad]3[pf]]xyz
Output: "adadpfpfpfadadpfpfpfadadpfpfpfxyz"
```

**Notice**:
```
Numbers can only appear in front of “[]”.
```

# Thoughts
I am going to use two stacks: push everything to stack1, and then pop elements one by one to stack2 so that the result will be in the right order.  There are four types of elements that we might meet: letter, digit, left bracket and right bracket.

Traversing the string, 
1. If the element visited is letter, push it to stack1. 
2. If the element visited is digit, push it to stack1 when left bracket is visited.
3. If the element visited is left bracket, push this number of duplication into the stack and reset number of duplication.
4. If the element visited is the right bracket, pop all the letters within the two brackets and use them to build a string, then push it back to stack1. 
5. Finally, push all the elements and use them to build a string and return the result.

# Difficulties:
1. The stack needs to contain several types of elements: int, string, and char, how to achieve it?

**Solution**: Use Stack<Object> stack. But do remember to cast stack.pop() to the proper type. If no cast is done, the type of stack.pop() will be  Object, resulting in imcompatile type error
  
2. How to pop the elements between left and right brackets? In other words, How to stop popping when stack.peek() is a number.

**Solution**: Use instanceof to check the type of element popped. Keep on popping as long as stack.peek() instanceof String && !stack.isEmpty

# Code (JAVA)
```java
public class Solution {
    /**
     * @param s: an expression includes numbers, letters and brackets
     * @return: a string
     */
    public String expressionExpand(String s) {
        // write your code here
        Stack<Object> stack = new Stack<>();
        char[] chars = s.toCharArray();
        
        // traverse chars and add element to stack
        int num = 0;
        for (char c : chars) {
            // if c represents a digit, record the value of it
            if (Character.isDigit(c)) {
                // the digit might be more than 10
                num = num * 10 + c - '0';
            } else {
                // if c is a left bracket
                if (c == '[') {
                    // push the value of digit into stack
                    stack.push(Integer.valueOf(num));
                    
                    // reset num
                    num = 0;
                } else {
                    // if c is a right bracket
                    if (c == ']') {
                        // pop all the letters after duplicate num to build a string
                        String newStr = popStack(stack);
                        
                        // push num of newStr back to stack
                        //System.out.println(stack.pop());
                        int duplicateNum = (Integer) stack.pop();
                        for (int i = 0; i < duplicateNum; i++) {
                            stack.push(newStr);
                        }
                    } else {
                        // if c is a letter, just push it to stack
                        stack.push(String.valueOf(c));
                    }
                }
            }
        }
        
        // pop stack to get the result
        String result = popStack(stack);
        return result;
    }
    
    private String popStack(Stack<Object> stack) {
        Stack<String> buffer = new Stack<>();
        
        // pop all elements until a int is found
        while (!stack.isEmpty() && stack.peek() instanceof String) {
            buffer.push((String) stack.pop());
        }
        
        StringBuilder sb = new StringBuilder();
        while (!buffer.isEmpty()) {
            sb.append(buffer.pop());
        }
        
        return sb.toString();
    }
}
```

# Complexity Analysis
**Time Complexity**: The worst case is that all the letters will be duplicated. Given the number of duplication as k and length of String s as n, the worst case time complexity will be O(kn). Therefore, this algorithm has a O(n) time complexity.

**Space Complexity**: The worst case is that all the letters will be duplicated. Given the number of duplication as k and length of String s as n, the worst case space complexity will be O(kn). Therefore, this algorithm has a O(n) space complexity.


