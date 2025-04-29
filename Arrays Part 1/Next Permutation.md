
[Problem Link](https://leetcode.com/problems/next-permutation/description/)

### Problem Statement : 
Given an array of integers `nums`, _find the next permutation of_ `nums`.

The replacement must be in place and use only constant extra memory.



### Approach 1 :

- Find all possible combinations i.e brute force then iterate over and find the given permutation and print the next one.
- Time Complexity : for first index if have n possibilities, for second i have n-1, for third n-2 and so on.
	- **Overall TC - O(n!)**
- Sort the string or array
```cpp
vector<string> ans;
void allPerm(string s,int i){
    if(i==s.size()-1){
        ans.push_back(s);
        return;
    }
    
    for(int j=i;j<s.size();j++){
        swap(s[i],s[j]);
        allPerm(s,i+1);    
    }
}
```

```
INPUT : ABC
OUTPUT : 
ABC
ACB
BAC
BCA
CAB
CBA
```

---

### Approach 2:


1. Let `arr = [2, 1, 5, 4, 3, 0, 0]`
2. Traverse from back and find first point where `arr[i] < arr[i+1]`
    
    - This identifies the element that needs to be replaced
3. Traverse from back again and find the element just greater than `arr[i]`
    
    - Looking for the next larger element to swap with
4. When `i` and `j` are identified:
    
    ```
    [2, 1, 5, 4, 3, 0, 0]
        i        j
    ```
    
5. Swap `arr[i]` and `arr[j]`
    
    - Result: `[2, 3, 5, 4, 1, 0, 0]`
6. After index `i`, sort in descending order, then reverse that portion
    
    - Final result: `[2, 3, 0, 0, 1, 4, 5]`
```cpp
void nextPermutation(vector<int>& nums) {
        // iterating from back and finding the pivot
        int n = nums.size();
        int index1 = -1;
        for(int i=n-1; i>0 ;i--){
            if(nums[i-1]<nums[i]){
                index1 = i-1;
                break;
            }
        }

        // find just greater element from nums[index1]
        int index2 = 0;
        for(int i=n-1; i>index1 && index1!=-1;i--){
            if(nums[i]>nums[index1]){
                index2 = i;
                break;
            }
        }

        //swapping numbers
        if(index1!=-1){
            swap(nums[index1],nums[index2]);
        }

        // reversing after pivot 
        reverse(nums.begin()+index1+1,nums.end());
    }
```

> `Time Complexity` : O(nlogn)
> 
> `Space Complexity` : O(1)

---
