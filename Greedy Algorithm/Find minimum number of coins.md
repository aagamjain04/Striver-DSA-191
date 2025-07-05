[Problem Link](https://www.geeksforgeeks.org/problems/-minimum-number-of-coins4426/1)
### Problem Statement : 

Given an **infinite supply** of each denomination of Indian currency { **1, 2, 5, 10, 20, 50, 100, 200, 500, 2000** } and a target value **N**.  
Find the **minimum** number of coins and/or notes needed to make the change for Rs **N**. You must return the list containing the value of coins required.

## Examples

**Example 1:**

```
Input: N = 43
Output: 20 20 2 1
Explaination: Minimum number of coins and notes needed to make 43.
```

### Approach 1 :

- Indian currency is a **canonical coin system**
- Each denomination is **at least 2x the previous** (mostly)
- **Greedy choice property** holds: taking the largest coin first always leads to optimal solution

#### Code :

``` cpp
vector<int> minPartition(int N) {
    vector<int> minCoins;
    vector<int> coinList = { 1, 2, 5, 10, 20, 50, 100, 200, 500, 2000 };
    
    int j = coinList.size() - 1;  // Start from largest denomination
    
    while(N > 0) {
        if(N < coinList[j]) {
            j--;  // Move to smaller denomination
        } else {
            N -= coinList[j];  // Use current denomination
            minCoins.push_back(coinList[j]);
        }
    }
    
    return minCoins;
}

```


> `Time Complexity` : O(denominations)
> 
> `Space Complexity` : O(1)


---


