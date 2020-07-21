# Problem
Lintcode 122. Largest Rectangle in Histogram

# Problem description
Given n non-negative integers representing the histogram's bar height where the width of each bar is 1, find the area of largest rectangle in the histogram.

**Example**:
```
Input：[2,1,5,6,2,3]
Output：10
Explanation：
The third and fourth rectangular truncated rectangle has an area of 2*5=10.
```

```
Input：[1,1]
Output：2
Explanation：
The first and second rectangular truncated rectangle has an area of 2*1=2.
```

**Notice**:
```

```
# Tool kits
Monotonic stack

# Thoughts
the height of a rectangle depends on the shortest bar in this area. Therefore, for the recentagle expanded from one certain bar, we need to find the first bar that is shorter than current bar on both left side and right side.The range between these two border bars are the maximum range given the height of current bar. <br/> We use a monotonically ascending stack to store the index of each bar. The reason why we store index rather than height is that We can get height based on index, but not vice versa. Whenever we find a height that is smaller than stack.peek(), in order to keep the stack monotonic, we need to kick the top element in the stack until stack.peek() is smaller than height. And we call the kicked bar as middle bar. In this way, we know the right border of the middle bar. The first bar on the right that is shorter than middle bar is exactly the bar with current index. The first bar on the left that is shorter than middle bar is 0 when the stack is empty, or stack.peek(). Given the height of middle bar as h, the range with h is [stack.peek() + 1, i - 1]. In this way we can calculate the area of each height and get the maxium one.


# Difficulties:
What if the heights of bars are in ascending trend so that no bar will be kicked and thus no middle bar will exist for calculation of area? <br/>
Solution: add -1 to the end of height. So that there will always be kicked bars.

# Code (JAVA)
```java
public class Solution {
    /**
     * @param height: A list of integer
     * @return: The area of largest rectangle in the histogram
     */
    public int largestRectangleArea(int[] height) {
        
        if (height == null || height.length == 0) {
            return 0;
        }
        
        Stack<Integer> stack = new Stack<>();
        int maxArea = 0;
        
        // [2]
        // [1]
        // [1,5]
        // [1,5,6]
        // [1,2]
        // [1,2,3]
        // curtHeigth = 2 < 6, h = 6, left = 3, right = 3
        // curtHeight = 2 < 5, h = 5, left = 2, right = 3
        
        for (int i = 0; i <= height.length; i++) {
            int curtHeight = i == height.length ? -1 : height[i];
            
            while (!stack.isEmpty() && curtHeight <= height[stack.peek()]) {
                int h = height[stack.pop()]; // the bar in the middle
                
                int left = stack.isEmpty() ? 0 : stack.peek() + 1;
                
                int right = i - 1;
                
                maxArea = Math.max(maxArea, h * (right - left + 1));
            }
            
            stack.push(i);
        }
        
        return maxArea;
    }
}
```

# Complexity Analysis
**Time Complexity**: O(n). In the worst case: such as height[2,3,4,5,6,7,1], time complexity is O(n - 1) * 2, which is still O(n) overall.

**Space Complexity**: O(n)

# Follow Up
