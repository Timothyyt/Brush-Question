# Problem
Lintcode 12. Min Stack

# Problem description
Implement a stack with following functions:

push(val) push val into the stack
pop() pop the top element and return it
min() return the smallest number in the stack
All above should be in O(1) cost.



**Example**:
```
Input:
  push(1)
  pop()
  push(2)
  push(3)
  min()
  push(1)
  min()
Output:
  1
  2
  1
```

```
Input:
  push(1)
  min()
  push(2)
  min()
  push(3)
  min()
Output:
  1
  1
  1
```

**Notice**:
```
min() will never be called when there is no number in the stack.
```
# Tool kits
Stack

# Thoughts
It is not ok to just use an integer minNum to record the smallest value. This is because, when the smallest value is popped from stack, it is hard to get the new smallest value, which is the second smallest value of the original stack. Therefore, a minStack that will always pop the smallest value is needed. When the smallest value in the stack is popped, minStack will also pop its top element to get updated.

# Difficulties:


# Code (JAVA)
```java
public class MinStack {
    Stack<Integer> stack;
    Stack<Integer> minStack;
    
    public MinStack() {
        // do intialization if necessary
        stack = new Stack<>();
        minStack = new Stack<>();
    }

    /*
     * @param number: An integer
     * @return: nothing
     */
    public void push(int number) {
        // the smallest number is always the top of minStack
        if (stack.isEmpty() || number <= minStack.peek()) {
            minStack.push(number);
        }
        
        stack.push(number);
    }

    /*
     * @return: An integer
     */
    public int pop() {
        // write your code here
        if (stack.isEmpty()) {
            return -1;
        }
        
        int num = stack.pop();
        
        if (num == minStack.peek()) {
            minStack.pop();
        }
        
        return num;
    }

    /*
     * @return: An integer
     */
    public int min() {
        // write your code here
        return minStack.peek();
    }
}

```

# Complexity Analysis
**Time Complexity**: All actions will take O(1) time

**Space Complexity**: Given the size of numbers added as n, the space complexity will be O(n).

# Follow Up
