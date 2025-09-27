[Problem Link](https://leetcode.com/problems/maximum-xor-with-an-element-from-array/description/)
### Problem Statement : 

Given:
- Array `nums` of non-negative integers
- Array `queries` where `queries[i] = [xi, mi]`

For each query, find:

- Maximum XOR of `xi` with any element in `nums` that is ≤ `mi`
- Return `-1` if no such element exists

**Example 1:**

```
Input: nums = [0,1,2,3,4], queries = [[3,1],[1,3],[5,6]]
Output: [3,3,7]
Explanation:
1) 0 and 1 are the only two integers not greater than 1. 0 XOR 3 = 3 and 1 XOR 3 = 2. The larger of the two is 3.
2) 1 XOR 2 = 3.
3) 5 XOR 2 = 7.
```

---

###  Approach 1 :

- Brute force
- For each query, iterate through all nums ≤ mi and compute the maximum XOR

> `Time Complexity` : O(queries * num) -> O(n * m)
> 
> `Space Complexity` : O(1)

---

### Approach 2:

- Trie approach
- **Sort queries** in increasing order of `m`.
- **Sort nums** (elements) in increasing order.
- For each query `(x, m)`:
    - Insert all numbers `≤ m` into the Trie.
    - Query Trie to find the **maximum XOR** with `x`.

#### Code :

``` cpp
class Solution {

struct Node{
    Node* bit[2] = {NULL};
};

Node* root;

void insert(vector<bool> &binary){

    Node* temp = root;
    for(int i=0;i<32;i++){
        int b = binary[i];
        if(temp->bit[b]==NULL){
            temp->bit[b] = new Node();
        }
        temp = temp->bit[b]; 
    }
}


int cal(vector<bool> &binary){

    int maxXor = 0;

    Node* temp = root;

    for(int i=0;i<32;i++){

        int val = binary[i];
        int req = 1-val;
        if(temp->bit[req]!=NULL){
            maxXor = maxXor | (1<<(31-i));
            temp = temp->bit[req];
        }else if(temp->bit[val]!=NULL){
            temp = temp->bit[val];
        }else{
            return -1;
        }
    }

    return maxXor;

}

public:
    vector<int> maximizeXor(vector<int>& nums, vector<vector<int>>& queries) {

        
          root = new Node();
          sort(nums.begin(),nums.end());
          vector<tuple<int,int,int>> q;

          int n = queries.size();
          for(int i=0;i<n;i++){
            q.push_back({queries[i][1],queries[i][0],i});
          }

          sort(q.begin(),q.end());

          vector<int> ans(n);

          int idx =0;

          // process offline queries
          for(int i=0;i<n;i++){
            int mi,xi,qidx;

            tie(mi,xi,qidx) = q[i];

            // insert all nums <= mi into trie

            while(idx < nums.size() && nums[idx]<=mi){
                //converting num to binary array
                vector<bool> binary(32);
                for(int j=0;j<32;j++){

                    int b = (1 << j) & nums[idx];
                    binary[31-j] = b;
                }

                //insert binary into trie
                insert(binary);
                idx++;
            }

            // if trie is not empty, cal max xor with xi
            //converting xi to binary array
                vector<bool> binary1(32);
                for(int j=0;j<32;j++){

                    int b = (1 << j) & xi;
                    binary1[31-j] = b;
                }
            if(idx>0)    
            ans[qidx] = cal(binary1);
            else
            ans[qidx] = -1;


          }

          return ans;
    }
};
```


> `Time Complexity` : O(nlogn + qlogq + q * 32)
> 
> `Space Complexity` : O(N * 32) for trie + (n + q)


---


