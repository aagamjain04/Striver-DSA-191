[Problem Link](https://www.geeksforgeeks.org/problems/find-nth-root-of-m5843/1#)
### Problem Statement : 

You are given 2 numbers **n and m,** the task is to find **n√m** (nth root of m). If the root is not integer then returns **-1**.
## Examples

**Example 1:**

```
Input: n = 4, m = 625
Output: 5
Explaination: 5^4 = 625

```

### Approach 1 :

- Using linear search
- We can guarantee that our answer will lie between the range from 1 to m i.e. the given number. So, we will perform a linear search on this range and we will find the number x, such that :
- func(x, n) = m. If no such number exists, we will return -1.

#### 

> `Time Complexity` : O(m)
> 
> `Space Complexity` : O(1)

---

### Approach 2:

- Using binary search combined with fast exponentiation
1. Initialize search space from `1` to `m`.
2. Use binary search to check the middle element `mid`.
3. Compare `mid^n` with `m`:
   - If equal: return `mid`.
   - If less: move to right half.
   - If greater: move to left half.
4. If no such integer found, return `-1`.



``` cpp
long poww(int x,int n){
	long res = 1;
	
	while(n>0){
		 if(n&1){
			res = res*x;
		}
		x = x*x;
		n = n >> 1;
	}
	return res;
  
}

int nthRoot(int n, int m) {
	
	int l = 1, r = m;
	while(l<=r){
	   int mid = (l+r)/2;
	   long res = poww(mid,n);
	   if(res==m){
		   return mid;
	   }else if(res > m){
		   r = mid-1;
	   }else{
		   l = mid+1;
	   } 
		
	}
	return -1;
	
}

```

> `Time Complexity` : O(log m x log n) -> outer binary search is log m and inner exponentiation per iteration is log n.
> 	
> `Space Complexity` : O(1)

---