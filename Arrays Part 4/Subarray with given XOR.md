[Problem Link](https://www.interviewbit.com/problems/subarray-with-given-xor/)

### Problem Statement : 
Given an array of integers **A** and an integer **B**.
Find the total number of subarrays having bitwise XOR of all elements equals to B.


## Example

**Example 1:**

```
Input: 
 A = [4, 2, 2, 6, 4]
 B = 6
Output: 4
Explanation: The subarrays having XOR of their elements as 6 are:
 [4, 2], [4, 2, 2, 6, 4], [2, 2, 6], [6]
```

### Approach 1 :

- Brute force
- Consider all possible sub arrays
- Calculate XOR of all sub arrays 
- if `XOR = B`, increment the count

```cpp
int solve(vector<int> &arr,int B){
	int n = arr.size();
	int ans = 0;

	for(int i = 0; i < n; i++){

		int xorr = arr[i];
        if(xorr==B)ans++;
		for(int j = i+1; j < n; j++){
			xorr = xorr ^ arr[j];
           
			if(xorr == B){
				ans++;
			}
		}

	}

	return ans;
}
```



> `Time Complexity` : O(n²)
> 
> `Space Complexity` : O(1)

---

### Approach 2  :

- XOR has the following property:

```
XOR(l, r] = Px[r] ⊕ Px[l]
```

Where `Px` represents **Prefix XOR array**


- We need to find subarray whose XOR is B

- If we want:

```
Px[r] ⊕ Px[l] = B
```

Then:

```
Px[r] ⊕ Px[l] ⊕ B = 0
```

Which gives us:

```
Px[l] ⊕ B = Px[r]
```

**Key Insight:** We need to find count of all `Px[l] ⊕ B` in hash-map.


- Maintain a running XOR (current XOR)
- Use hash map to store frequency of XOR seen so far

### For each element:
- **Check:** if `current XOR ⊕ B` exists in hash table, increment counter by `freq[k]`
- **Update:** hash with current XOR

## Key Points

If `Px[r] ⊕ B = k`, then we need to look for sum `Px[l] = k`, since that range will have XOR B.


## Implementation Notes

- Initialize hash-map with `{0: 1}` to handle subarrays starting from index 0
- Process elements left to right
- For each element, first check for valid subarrays ending at current position
- Then update hash-map with current prefix XOR


``` cpp
int solve(vector<int> &arr, int B) {
    
    int n = arr.size();
    vector<int> px(n);
    unordered_map<int,int> mp;
    int ans = 0;
    mp[0] = 1;
    px[0] = arr[0];
    mp[px[0]]++;
    if(px[0]==B){
        ans++;
    }
    for(int i=1;i<n;i++){
        px[i] = px[i-1] ^ arr[i];
        
        int k = px[i]^B;
        if(mp.find(k)!=mp.end()){
            ans+=mp[k];
        }
        mp[px[i]]++;
    }
    
    return ans;

}
```


> `Time Complexity` : O(n) , considering O(1) for unordered map
> 
> `Space Complexity` : O(n) , for hash-map

---
