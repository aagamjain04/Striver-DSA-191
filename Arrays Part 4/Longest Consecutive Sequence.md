[Problem Link](https://leetcode.com/problems/longest-consecutive-sequence/description/)

### Problem Statement : 
Given an unsorted array of integers `nums`, return _the length of the longest consecutive elements sequence_.

## Example

**Example 1:**
```
Input: nums = [100,4,200,1,3,2]
Output: 4
Explanation: The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.
```

**Example 2:**
```
Input: nums = [1,0,1,2]
Output: 3
```

### Approach 1 :

- For every element `x` look for `x+1` in the array.

```
E.g 
nums[] = [1,2,3,4]

1->2->3->4 = 4
2->3->4 = 3
3->4 = 2
4 = 1

So ans is max of all i.e 4
```

> `Time Complexity` : O(n²)
> 
> `Space Complexity` : O(1)


---

### Approach 2:

- Sort the array
- skip when `a[i]==a[i-1]`
- increment count when `a[i]-a[i-1]=1`, if not make count 0
- Return the max count

> `Time Complexity` : O(nlogn)
> 
> `Space Complexity` : O(1)

---

### Approach 3:

- In brute force we look for `x+1` for all `x`, to overcome this we look for `x+1` for only those element whose `x-1` is not present.
- So that `x` will eventually be starting of sequence
- First add all elements in unordered set
- ~~Now iterate over array~~ , **this give TLE on leet-code**
- **Iterate through the set instead of the original array** - This avoids checking duplicate numbers multiple times, which was causing the TLE.
- Now iterate over set elements
- if `x-1` exists, move to next element
- if `x-1` does not exist, look for x+1 in set and x+2,x+3...


>`Time Complexity` : O(N) + O(2*N) ~ O(3*N), where N = size of the array.  
>Reason: O(N) for putting all the elements into the set data structure. After that for every starting element, we are finding the consecutive elements. Though we are using nested loops, the set will be traversed at most twice in the worst case. So, the time complexity is `O(2*N) `instead of O(N2).
>
>`Space Complexity` : O(N), as we are using the set data structure to solve this problem.

**Note:** The time complexity is computed under the assumption that we are using unordered_set and it is taking O(1) for the set insert and search operations. 

- If we consider the worst case the set operations will take O(N) in that case and the total time complexity will be approximately O(N2). 
- And if we use the set instead of unordered_set, the time complexity for the set operations will be O(logN) and the total time complexity will be O(NlogN).

### Code :

``` cpp
 int longestConsecutive(vector<int>& nums) {
        
	int n = nums.size();
	int ans = 0;
	unordered_set<int> s(nums.begin(),nums.end());
	for(auto i:s){

		int count = 1;
		int x = i;
		if(s.find(x-1)==s.end()){
			while(s.find(x+1)!=s.end())
			{
				x++;
				count++;
			}

			ans = max(ans,count);
		}
	}

	return ans;

}
```


---
