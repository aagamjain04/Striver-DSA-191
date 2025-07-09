[Problem Link](https://leetcode.com/problems/permutation-sequence/description/)
### Problem Statement : 

Given **N** and **K**, where N is the sequence of numbers from 1 to `N([1,2,3..... N])` find the **Kth permutation sequence**.
## Examples

**Example 1:**

```
Input: n = 3, k = 3
Output: "213"
Explanation: 
1. 123
2. 132
3. 213
4. 231
5. 312
6. 321
```

### Approach 1 :

- Use **backtracking** to generate all possible permutations of the string `"123...n"` and return the (k-1)-th (0-indexed) permutation.`

#### Code :

``` cpp
void recur(int i,string s,int n,vector<string> &ans){

	if(i==n){
	   ans.push_back(s);
	   return;
	}

	for(int j=i;j<n;j++){
		swap(s[i],s[j]);
		recur(i+1,s,n,ans);
		// No need to swap back as 's' is passed by value

	}
}

string getPermutation(int n, int k) {
	
	vector<string> ans;
   
	string s = "";
	for(int i=1;i<=n;i++){
		s+=(i+'0');
	}

	recur(0,s,n,ans);

	return ans[k-1];
}
```


> `Time Complexity` : O(n × n!) -> n! is number of recursive calls and n is string s created in every call as its not passed by reference.
> 
> `Space Complexity` : `O(n × n!)` (each permutation is length `n`)

---

### Approach 2:

- Permutations can be grouped by their first digit
- Each group has (n-1)! permutations
- We can calculate which group contains the k-th permutation
- Then recursively find the position within that group


```
let n = 4

Block size = 6
[ 1234, 1243, 1324, 1342, 1423, 1432 ]   <- Block 0 (starts with 1)
[ 2134, 2143, 2314, 2341, 2413, 2431 ]   <- Block 1 (starts with 2)
[ 3124, 3142, 3214, 3241, 3412, 3421 ]   <- Block 2 (starts with 3)
[ 4123, 4132, 4213, 4231, 4312, 4321 ]   <- Block 3 (starts with 4)

let k = 14 res = ("3142")

-> k = 13 (Convert to 0-indexed)
-> fact = (n-1)!
-> block = k/fact = 13/6 = 2
-> result lies in 3rd block
-> iterate again with smaller scope

```

``` cpp
string getPermutation(int n, int k) {
	
	int fact = 1;
	vector<int> nums;
	for(int i=1;i<n;i++){
		nums.push_back(i);
		fact = fact*i; // Calculate (n-1)!
	}
	nums.push_back(n);

	k--; // Convert to 0-indexed

	string res = "";
	int d = n-1;

	while(true){

		int block = k/fact; // Which group?
		res+= (nums[block]+'0'); // Add the digit

		k = k - (block*fact); // Remaining position within group

		nums.erase(nums.begin()+block); // Reomove used digit
		if(d==0)
		break;
		fact = fact/d; // Update factorial for next level
		d--; // Decrement remaining digit

	}

	return res;

}

```

> `Time Complexity` : O(N x N) -> loop run N times and for every loop vector erase is O(N)
> 	
> `Space Complexity` : O(N) (`nums` array)


---