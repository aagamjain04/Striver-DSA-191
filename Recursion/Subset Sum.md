[Problem Link](https://www.geeksforgeeks.org/problems/subset-sums2234/1)
### Problem Statement : 

Given a array **`arr`** of integers, return the sums of all subsets in the list.  Return the sums in any order.
## Examples

**Example 1:**

```
Input: arr[] = [2, 3]
Output: [0, 2, 3, 5]
Explanation: When no elements are taken then Sum = 0. When only 2 is taken then Sum = 2. When only 3 is taken then Sum = 3. When elements 2 and 3 are taken then Sum = 2+3 = 5.
```

### Approach 1 :

- Using recursion
- The main idea is that on every index you have two options either to select the element to add it to your subset(pick) or not select the element at that index and move to the next index(non-pick).

#### Code :

``` cpp
 void recur(int i,vector<int> &arr,int sum,vector<int> &res){
        
	if(i>=arr.size()){
		res.push_back(sum);
		return;
	}
	
	recur(i+1,arr,sum+arr[i],res);
	recur(i+1,arr,sum,res);
	
}
  
vector<int> subsetSums(vector<int>& arr) {
	vector<int> res;
	recur(0,arr,0,res);
	return res;
}

```


> `Time Complexity` : O(2^n)
> 
> `Space Complexity` : O(n) auxiliary space for recursion stack (not counting output array)


---

### Approach 2 :

- Using bit masking
- Every subset of the array can be represented using a binary number of length `n`, where `n = arr.size()`.
- A bit set to `1` at position `j` indicates that `arr[j]` is included in the current subset.
- We iterate through all numbers from `0` to `(2^n - 1)` to generate all subsets and compute their sums.

#### Code :

``` cpp

vector<int> subsetSums(vector<int>& arr) {
    int n = arr.size();
    vector<int> res;

    for(int i = 0; i < (1 << n); i++) {
        int sum = 0;

        for(int j = 0; j < n; j++) {
            if(i & (1 << j)) {
                sum += arr[j];
            }
        }

        res.push_back(sum);
    }

    return res;
}

```

> `Time Complexity` : O(n * 2^n)
> 
> `Space Complexity` : O(1) (not counting output array)

---
