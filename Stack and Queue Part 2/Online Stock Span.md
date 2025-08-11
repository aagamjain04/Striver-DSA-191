[Problem Link](https://leetcode.com/problems/online-stock-span/description/)
### Problem Statement : 

Design an algorithm that collects daily price quotes for some stock and returns **the span** of that stock's price for the current day.

The **span** of the stock's price in one day is the maximum number of consecutive days (starting from that day and going backward) for which the stock price was less than or equal to the price of that day.

**Example 1:**

```
Input
["StockSpanner", "next", "next", "next", "next", "next", "next", "next"]
[[], [100], [80], [60], [70], [60], [75], [85]]
Output
[null, 1, 1, 1, 2, 1, 4, 6]

Explanation
StockSpanner stockSpanner = new StockSpanner();
stockSpanner.next(100); // return 1
stockSpanner.next(80);  // return 1
stockSpanner.next(60);  // return 1
stockSpanner.next(70);  // return 2
stockSpanner.next(60);  // return 1
stockSpanner.next(75);  // return 4, because the last 4 prices (including today's price of 75) were less than or equal to today's price.
stockSpanner.next(85);  // return 6
```

### Approach 1 :
- Brute force
- For each new price, iterate backward through all previous prices until we find a price greater than the current price.

#### Code :

```cpp

class StockSpanner {
private:
    vector<int> prices;
    
public:
    StockSpanner() {
        // Initialize empty price history
    }
    
    int next(int price) {
        prices.push_back(price);
        int span = 1; // At least today counts
        
        // Look backward from yesterday
        for (int i = prices.size() - 2; i >= 0; i--) {
            if (prices[i] <= price) {
                span++;
            } else {
                break; // Found a price greater than today's price
            }
        }
        
        return span;
    }
};
```

> `Time Complexity` : O(n) per query, O(n²) overall
> 
> `Space Complexity` : O(n) 


---

###  Approach 2 :

- Monotonic stack
- For each new price, iterate backward through all previous prices until we find a price greater than the current price.
- Stack is **monotonic decreasing** in price.
- When a new price comes in:
    - Pop elements from stack while `price >= stack.top().price`
    - Add their spans to current day's span.
- Push `(price, span)` back to stack.

**Why it works:**

- If today's price is higher or equal to a past day’s price, that past day's span can be combined into today’s span.
    
- This avoids re-checking individual days repeatedly.

#### Code :

```cpp

class StockSpanner {
public:

    stack<pair<int,int>> st;
    
    StockSpanner() {
        
    }
    int next(int price) {
        int span = 1;
        while(!st.empty() && price >= st.top().first){
            span+=st.top().second;
            st.pop();
        }
        st.push({price,span});
        return span;

    }
};

```


> `Time Complexity` : O(1) amortized per query
> 
> `Space Complexity` : O(n)

---

