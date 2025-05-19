[Problem Link](https://www.geeksforgeeks.org/problems/inversion-of-array-1587115620/1)

### Problem Statement : 
Given an array of integers `arr[]`. Find the **Inversion Count** in the array.  
Two elements `arr[i]` and `arr[j]` form an inversion if `arr[i] > arr[j]` and `i < j`.

## Example

**Example 1:**

```
Input: arr = [2,5,1,3,4]
Output: 4
Explanation: (2,1) (5,1) (5,3) (5,4)
```


### Approach 1 :

**Brute force** : Run a nested loop and check all pairs where `i<j` and `arr[i]>arr[j]`.
Return the count of such pairs.

> `Time Complexity` : O(n²)
> 
> `Space Complexity` : O(1)

---
### Approach 2 :

- **Fenwick Tree**
- Given `n < 10⁵` and `arr[i]<10⁵`

- **Logic** :
- We will create one BIT array and one by one will update the count in BIT. Now before updating , we will check the count of elements greater than current element in BIT. That's our inversion. Return total count.

#### Finding the rightmost set bit of number x :

Let `x = a 1 b` (where pattern is `a 1 (000..00)`)

- `-x = 2's cpmplement of x`
- ` = (x') + 1`
- ` = a' 0 (111..111) `

Therefore,
- `x = a 1 b`
- `-x = a' 0 b'`
- `x & (-x) = 0 1 0` 


<div style="border:1px solid #ccc; padding:10px">
<b>Rightmost set bit for any number x = x & (-x)</b><br>
<b>To remove last set bit : x = x - (x & (-x))</b>
</div>


## BIT Array Structure

```
BIT = [0, 1, 2, 3, 4, 5, 6, ..., 10, 11, 12, 13]
```

- `i → number`
- `j → remove last bit i`

**Note**: Any index i will store sum from j+1 to i

## Examples

**13 = 1101 ⇒ LBR = 1100 (12)**

- `BIT[13] = 13 → 13`

**12 = 1100 ⇒ LBR = 1000 (8)**

- `BIT[12] = 9 → 12`

**8 → 1000 ⇒ LBR = 0**

- `BIT[8] = 1 → 8`

**Sum(1, 13) = BIT[13] + BIT[12] + BIT[8]**

## Operations

### Sum Function

**Find sum from 1 to a number x**

```cpp
void sum(int x) {
    int sum = 0;
    while (x > 0) {
        sum += BIT[x];
        x = x - (x & (-x));
    }
    return sum;
}
```

**Time Complexity**: O(log n)

### Update Function

**Updating value v at index x, we need to update all index whose range has x**



```cpp
void update(int x, int v) {
    while (x < N) {          // N = 10^5
        BIT[x] += v;
        x = x + (x & (-x));
    }
}
```

**Time Complexity**: O(log n)

### Code :

```cpp
class Solution {
  public:
    // Function to count inversions in the array.
    const int N = 100001;
    vector<int> BIT;
    //calculates sum from 0 to i
    int sum(int i){
        int cnt = 0;
        while(i>0){
            cnt+=BIT[i];
            i-=(i&(-i));
        }
        return cnt;
    }
    
    void update(int i){
        while(i<N){
            BIT[i]+=1;
            i+=(i&(-i));
        }
        
    }
    
    int inversionCount(vector<int> &arr) {
        BIT.assign(N,0);
        
        int ans = 0;
        
        int n = arr.size();
        for(int i=0;i<n;i++){
            int x = arr[i];
            
            ans+=(sum(N)-sum(x));
            update(x);
            
        }
        return ans;
    }
};

```


---


