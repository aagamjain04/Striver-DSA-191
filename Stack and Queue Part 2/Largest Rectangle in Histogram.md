[Problem Link](https://leetcode.com/problems/largest-rectangle-in-histogram/description/)
### Problem Statement : 

Given an array of integers `heights` representing the histogram's bar height where the width of each bar is `1`, return _the area of the largest rectangle in the histogram_.

 ![img](../Images/histogram.jpg)

**Example 1:**

```
Input: heights = [2,1,5,6,2,3]
Output: 10
Explanation: The above is a histogram where width of each bar is 1.
The largest rectangle is shown in the red area, which has an area = 10 units.

```

### Approach 1 :
- Brute force
- For every bar `i`, expand left and right until we find a bar shorter than `heights[i]`.
- Calculate the width and area for each expansion.
- Keep track of the max area.

> `Time Complexity` : O(n²)
> 
> `Space Complexity` : O(1) 


---

###  Approach 2 :

- Using Next Smaller Element (NSE)
-  Precompute:
    - `left[i]`: index of first smaller element to the **left** of `i`.
    - `right[i]`: index of first smaller element to the **right** of `i`.
- Width = `right[i] - left[i] - 1`.
- Area = `heights[i] × width`.


#### Code :

```cpp
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int n = heights.size();

        stack<int> st;

        vector<int> left(n);
        vector<int> right(n);
        int ans = 0;
        for(int i=0;i<n;i++)
        {
            while(!st.empty() && heights[st.top()]>=heights[i])
            {
                st.pop();
            }
            
            int l = (st.empty())? -1 : st.top();

            st.push(i);

            left[i] = l;
        }

        // for(auto i:left)
        // cout << i << " ";

        while(!st.empty())
        st.pop();

        for(int i=n-1;i>=0;i--)
        {
           
            while(!st.empty() && heights[st.top()]>=heights[i])
            {
                st.pop();
            }
            
            int r = (st.empty())? n : st.top();

            st.push(i);

            right[i] = r;
        }
        
        for(int i=0;i<n;i++)
        {
            ans = max(ans,(right[i]-left[i]-1) * heights[i]);
            
        }
        

        return ans;
    }
};
```


> `Time Complexity` : O(n) 
> 
> `Space Complexity` : O(n)

---

### Approach 3 :

- Maintain increasing monotonic stack
- Append a `0` to `heights` so all bars are processed.
- Initialize empty stack.
- Traverse array:
    - While stack not empty and current height < height at stack top:
        - Pop from stack.
        - Calculate width.
        - Update max area.
    - Push current index to stack.
- Return max area.

#### Code :


```cpp
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        
        heights.push_back(0);
        int n = heights.size();
        stack<int> st;

        int ans = 0;

        for(int i=0;i<n;i++){


            while(!st.empty() && heights[i] < heights[st.top()]){

                
                int h = heights[st.top()];

                st.pop();

                int l = st.empty() ? -1 : st.top(); 
                int r = i;

                ans = max(ans,(r-l-1) * h);
            
            }

           
            st.push(i);

        }
        return ans;
    }
};

```


> `Time Complexity` : O(n) 
> 
> `Space Complexity` : O(n)

---
