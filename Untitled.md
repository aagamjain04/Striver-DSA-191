# Best Time to Buy and Sell Stock

## Problem Statement

Given an array `prices` where `prices[i]` is the price of a stock on the `ith` day:

- You can buy one stock on a single day
- You can sell it on a different day in the future
- Return the maximum profit possible from this transaction
- Return 0 if no profit can be achieved

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

## Approach 1: Brute Force

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

## Approach 2: One Pass (Optimized)

- Keep track of the minimum price seen so far
- Calculate potential profit at each step
- Time complexity: O(n)
- Space complexity: O(1)

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

## Key Insights

1. We only need to make a single pass through the array
2. At each step, we want to know:
    - What's the minimum price we've seen so far?
    - What's the maximum profit we can get by selling at the current price?
3. The optimal solution requires finding the largest price difference where the smaller price comes before the larger price

## Edge Cases

- Empty array: Return 0
- Continuously decreasing prices: Return 0
- Single element array: Return 0 (can't both buy and sell)

## Complexity Analysis

- Time Complexity: O(n) - we only need to iterate through the prices array once
- Space Complexity: O(1) - we only use a constant amount of extra space