[Problem Link](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/)

### Problem Statement : 

You are given an array of prices where `prices[i]` is the price of a given stock on an ith day.  
\
You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock. Return _the maximum profit you can achieve from this transaction_. If you cannot achieve any profit, return 0.

## Examples

```
Input: prices = [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
```

```
Input: prices = [7,6,4,3,1]
Output: 0
Explanation: No profit can be achieved as prices are continuously decreasing.
```

### Approach 1: Brute Force

- Check all possible buying and selling combinations
- Time complexity: O(n²)
- Space complexity: O(1)

```cpp
int maxProfit(vector<int>& prices) {
    int maxProfit = 0;
    int n = prices.size();
    
    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            int profit = prices[j] - prices[i];
            maxProfit = max(maxProfit, profit);
        }
    }
    
    return maxProfit;
}
```

---
### Approach 2: One Pass (Optimized)

- Keep track of the minimum price seen so far
- Calculate potential profit at each step


```cpp
int maxProfit(vector<int>& prices) {
    if (prices.empty()) return 0;
    
    int minPrice = INT_MAX;
    int maxProfit = 0;
    
    for (int price : prices) {
        // Update minimum price seen so far
        minPrice = min(minPrice, price);
        
        // Calculate potential profit if we sell at current price
        int potentialProfit = price - minPrice;
        
        // Update max profit if current potential profit is greater
        maxProfit = max(maxProfit, potentialProfit);
    }
    
    return maxProfit;
}
```


> `Time Complexity` : O(n)
> 
> `Space Complexity` : O(1)

---