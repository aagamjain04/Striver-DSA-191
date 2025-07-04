[Problem Link](https://www.geeksforgeeks.org/problems/fractional-knapsack-1587115620/1)
### Problem Statement : 

Given two arrays, `val[]` and `wt[]`, representing the values and weights of items, and an integer `capacity` representing the maximum weight a knapsack can hold, determine the maximum total value that can be achieved by putting items in the knapsack. You are allowed to break items into fractions if necessary.

## Examples

**Example 1:**

```
Input: val[] = [60, 100, 120], wt[] = [10, 20, 30], capacity = 50
Output: 240.000000
Explanation: Take the item with value 60 and weight 10, value 100 and weight 20 and split the third item with value 120 and weight 30, to fit it into weight 20. so it becomes (120/30)*20=80, so the total value becomes 60+100+80.0=240.0 Thus, total maximum value of item we can have is 240.00 from the given capacity of sack.
```

### Approach 1 :

- Unlike 0/1 knapsack, we can take `fractions` of items.
- Sort items by value-to-weight ratio in descending order and greedily pick items with the highest ratio first.
##### Why Greedy Works for Fractional Knapsack?

- We can take fractions of items, so we should prioritize items that give maximum value per unit weight
- Taking the item with highest value/weight ratio first is always optimal
- If we can't fit the entire item, we take the fraction that fits



#### Code :

``` cpp
// Custom comparator to sort by value/weight ratio in descending order
static bool comp(pair<int,int> &a, pair<int,int> &b) {
	double ratioA = (double)a.first / (double)a.second;
	double ratioB = (double)b.first / (double)b.second;
	return ratioA > ratioB;
}

double fractionalKnapsack(vector<int>& val, vector<int>& wt, int capacity) {
	int n = val.size();
	vector<pair<int,int>> items;
	
	// Create (value, weight) pairs
	for(int i = 0; i < n; i++) {
		items.push_back({val[i], wt[i]});
	}
	
	// Sort by value/weight ratio in descending order
	sort(items.begin(), items.end(), comp);
	
	double totalValue = 0;
	
	for(int i = 0; i < n; i++) {
		int currVal = items[i].first;
		int currWt = items[i].second;
		
		if(currWt <= capacity) {
			// Take the whole item
			totalValue += (double)currVal;
			capacity -= currWt;
		} 
		else if(capacity > 0) {
			// Take fraction of the item
			double fraction = (double)capacity / (double)currWt;
			totalValue += ((double)currVal * fraction);
			capacity = 0;  // Knapsack is full
		}
	}
	
	return totalValue;
}

```


> `Time Complexity` : O(nlogn)
> 
> `Space Complexity` : O(1)


---


