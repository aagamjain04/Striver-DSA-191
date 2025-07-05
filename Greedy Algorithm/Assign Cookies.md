[Problem Link](https://leetcode.com/problems/assign-cookies/description/)
### Problem Statement : 

Assume you are an awesome parent and want to give your children some cookies. But, you should give each child at most one cookie.

Each child `i` has a greed factor `g[i]`, which is the minimum size of a cookie that the child will be content with; and each cookie `j` has a size `s[j]`. If `s[j] >= g[i]`, we can assign the cookie `j` to the child `i`, and the child `i` will be content. Your goal is to maximize the number of your content children and output the maximum number.

## Examples

**Example 1:**

```
Input: g = [1,2,3], s = [1,1]
Output: 1
Explanation: You have 3 children and 2 cookies. The greed factors of 3 children are 1, 2, 3. 
And even though you have 2 cookies, since their size is both 1, you could only make the child whose greed factor is 1 content.
```

### Approach 1 :

- The optimal approach is to use a greedy algorithm:
- If we can't satisfy a less greedy child with a cookie, we definitely can't satisfy a more greedy child with the same cookie
- By using the smallest possible cookie for each child, we save larger cookies for children who need them

#### Code :

``` cpp
int findContentChildren(vector<int>& g, vector<int>& s) {
        // Sort both arrays
        sort(g.begin(), g.end());
        sort(s.begin(), s.end());
        
        int child = 0;
        int cookie = 0;
        int count = 0;
        
        // Two pointer approach
        while (child < g.size() && cookie < s.size()) {
            if (s[cookie] >= g[child]) {
                // Cookie satisfies child
                count++;
                child++;  // Move to next child
            }
            cookie++;  // Always move to next cookie
        }
        
        return count;
    }

```


> `Time Complexity` : O(n log n + m log m)
> 
> `Space Complexity` : O(1)


---


