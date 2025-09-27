[Problem Link](https://leetcode.com/problems/maximum-xor-of-two-numbers-in-an-array/description/)
### Problem Statement : 

- Given an integer array `nums`, return _the maximum result of_ `nums[i] XOR nums[j]`, where `0 <= i <= j < n`.


**Example 1:**

```
Input: nums = [3,10,5,25,2,8]
Output: 28
Explanation: The maximum result is 5 XOR 25 = 28.
```

---

###  Approach 1 :

- Brute force
- Try all possible pairs `(i, j)` and compute `nums[i] ^ nums[j]`.
- Keep track of the maximum.

> `Time Complexity` : O(N^2)
> 
> `Space Complexity` : O(1)

---

### Approach 2:

- Trie approach
- **Build a Trie** of all numbers:
    - Each node has two children: `0` and `1`.
- **Query Trie** for each number:
    - For each bit, move toward the opposite bit if available.
    - Accumulate XOR result.
- Keep track of the **maximum XOR found**.

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
        }else{
            temp = temp->bit[val];
        }
    }

    return maxXor;

}


public:
    int findMaximumXOR(vector<int>& nums) {
        
        root = new Node();

        int ans = 0;

        for(auto num : nums){

            //converting num to binary array
            vector<bool> binary(32);
            for(int i=0;i<32;i++){

                int b = (1 << i) & num;
                binary[31-i] = b;
            }

            //insert binary into trie
            insert(binary);

            //calculate max XOR
            ans = max(cal(binary),ans);
        }


        return ans;

    }
};
```


> `Time Complexity` : O(N * B)
> 
> `Space Complexity` : ON * B) for trie


---


