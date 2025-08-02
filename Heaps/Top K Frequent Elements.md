[Problem Link](https://leetcode.com/problems/top-k-frequent-elements/description/)
### Problem Statement : 

Given an integer array `nums` and an integer `k`, return _the_ `k` most frequent elements. You may return the answer in **any order**.
## Examples

**Example 1:**

```
Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]
```

### Approach 1 :

- Hash Map + Sorting
- Count frequency of each element using hash map
- Convert to vector of pairs (element, frequency)
- Sort by frequency in descending order
- Take first k elements


> `Time Complexity` : O(N log N)
> 	
> `Space Complexity` : O(N)

---

### Approach 2 :

- Min Heap Approach
- Count frequencies using unordered hash map
- Use min heap of size k
- For each element, add to heap if heap size < k, or replace minimum if current frequency is larger
- Extract all elements from heap

#### Code :

```cpp
vector<int> topKFrequent(vector<int>& nums, int k) {
	// Step 1: Count frequencies
	unordered_map<int, int> freq;
	for (int num : nums) {
		freq[num]++;
	}
	
	// Step 2: Min heap of pairs (frequency, element)
	priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> minHeap;
	
	// Step 3: Process each unique element
	for (auto& p : freq) {
		minHeap.push({p.second, p.first});
		
		// Keep only k elements in heap
		if (minHeap.size() > k) {
			minHeap.pop();
		}
	}
	
	// Step 4: Extract elements from heap
	vector<int> result;
	while (!minHeap.empty()) {
		result.push_back(minHeap.top().second);
		minHeap.pop();
	}
	
	return result;
}

```


> `Time Complexity` : O(N log k)
> 	
> `Space Complexity` : O(n+k)

---

### Approach 3 :

- Bucket Sort
- Count frequencies
- Create buckets where index represents frequency
- Place elements in corresponding frequency buckets
- Traverse buckets from high to low frequency, collect k elements

#### Code :

```cpp

vector<int> topKFrequent(vector<int>& nums, int k) {
	
	int n = nums.size();
	unordered_map<int,int> mp;

	for(auto &i: nums){
		mp[i]++;
	}

	vector<vector<int>> bucket(n+1);

	for(auto i: mp){
		int num = i.first;
		int fre = i.second;

		bucket[fre].push_back(num);
	}

	vector<int> res;

	for(int i=n;i>=0;i--){

		for(auto j:bucket[i]){
			res.push_back(j);
			if(res.size()==k)
			break;
		}

		if(res.size()==k)
		break;

	}
	
	return res;
}

```

> `Time Complexity` : O(n)
> 	
> `Space Complexity` : O(n)

---
