[Problem Link](https://leetcode.com/problems/reverse-nodes-in-k-group/description/)
### Problem Statement : 

Given the `head` of a linked list, reverse the nodes of the list `k` at a time, and return _the modified list_.

`k` is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of `k` then left-out nodes, in the end, should remain as it is.

You may not alter the values in the list's nodes, only nodes themselves may be changed.

#### Example 1 : 

```
Input:  1 -> 2 -> 3 -> 4 -> 5, k = 2
Output: 2 -> 1 -> 4 -> 3 -> 5
```

#### Example 2 : 

```
Input:  1 -> 2 -> 3 -> 4 -> 5, k = 3
Output: 3 -> 2 -> 1 -> 4 -> 5
```

### Approach 1:

1. **Check if `k` nodes are available** from the current head:
   - Traverse `k` steps ahead.
   - If less than `k` nodes remain, return head.

2. **Reverse `k` nodes**:
   - Use three pointers: `prev`, `curr`, and `next` to reverse links.

3. **Recursive call**:
   - After reversing `k` nodes, recursively process the remaining list.

4. **Link the current reversed group** to the result of the next recursive call.

```cpp
 ListNode* reverseKGroup(ListNode* head, int k) {
        
	ListNode* temp = head;
	int cnt = 0;

	while(temp && cnt<k){
		temp = temp->next;
		cnt++;
	}
  
	if(cnt<k){
		return head;
	}

	ListNode* curr = head;
	ListNode* prev = NULL;
	ListNode* next;
	cnt = 0;
	while(curr && cnt<k){
		cnt++;
		next = curr->next;
		curr->next = prev;
		prev = curr;
		curr = next;
	}
	
	head->next = reverseKGroup(temp,k);

	return prev;
}

```


> `Time Complexity` : O(N)
> 
> `Space Complexity` : O(N/k) - due to recursion stack

---

