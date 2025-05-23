
[Problem Link](https://leetcode.com/problems/reverse-pairs/description/)

Given an array of numbers, you need to return the count of reverse pairs. Reverse Pairs are those pairs where `i<j` and `arr[i]>2*arr[j]`.

## Example

**Example 1:**

```
Input: arr = [2,4,3,5,1]
Output: 3
Explanation: The reverse pairs are:
(1, 4) --> nums[1] = 4, nums[4] = 1, 4 > 2 * 1
(2, 4) --> nums[2] = 3, nums[4] = 1, 3 > 2 * 1
(3, 4) --> nums[3] = 5, nums[4] = 1, 5 > 2 * 1
```


### Approach 1 :

**Brute force** : Run a nested loop and check all pairs where `i<j` and `arr[i]>2*arr[j]`.
Return the count of such pairs.

> `Time Complexity` : O(n²)
> 
> `Space Complexity` : O(1)


---

### Approach 2:

- Similar to count inverse problem , but only variation here is instead of `arr[i] > arr[j]` we have `arr[i] > 2 * arr[j]`
- So we will use merge sort logic
- And just before merging two sorted arrays we need to find count

- Let `arr1 = [1,4,6,7]` and `arr2 = [2,3,4,5]`
- Here we cannot use same logic of inversion count, since merge functions works comparing two elements `arr1[i] < arr2[j]`. But here our condition is different and if we change condition we may not have sorted array.
- Idea is to have a logic before merging two array in linear time.

- Count in above example : (6,2) (7,2) (7,3)
- (6,2) made pair and now pointer moves to (6,3) which does not make pair.
- Now 7 will start checking from 3 and (7,3) will make pair and also all numbers before 3 makes pair.

#### Code :


```cpp
class Solution {
public:

    int merge(int l,int r,int m,vector<int>& nums)
    {
        int n1 = m-l+1;
        int n2 = r-m;

        vector<int> arr1(n1),arr2(n2);
      
        for(int i=0;i<n1;i++)
        arr1[i] = nums[l+i];
           
        for(int i=0;i<n2;i++)
        arr2[i] = nums[m+1+i];
           
    
        int count = 0;
        int a = 0;
		// Count logic start
        for(int i=0;i<n1;i++){
            while(a<n2 && (arr1[i]>2LL * arr2[a])){
                a++;
            }
            count+=a;
        }
        // Count logic end

        int j=0,k=0;
        int i = l;
        while(j<n1 && k<n2){
            if(arr1[j]<arr2[k]){
                nums[i] = arr1[j];
                i++;j++;
            }
            else{
                nums[i] = arr2[k];
                i++;k++;
            }
        
        }

        while(j<n1){
            nums[i] = arr1[j];
            i++;j++;
        }

        while(k<n2){
            nums[i] = arr2[k];
            i++;k++;
        }

        return count;

    }


    int mergeSort(int l,int r,vector<int> &nums)
    {
        int count = 0;
        if(l<r)
        {
            int m = (l+r)/2;
            count+=mergeSort(l,m,nums);
            count+=mergeSort(m+1,r,nums);
            count+=merge(l,r,m,nums);
        }
        return count;
    }

    int reversePairs(vector<int>& nums) {
        
        int n = nums.size();

        return mergeSort(0,n-1,nums);

    }
};
```


> `Time Complexity` : O(NlogN)
> 
> `Space Complexity` : O(N) (Temp array in merge sort)

---
