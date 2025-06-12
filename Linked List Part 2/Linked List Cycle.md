[Problem Link](https://leetcode.com/problems/linked-list-cycle/description/)
### Problem Statement : 

Given `head`, the head of a linked list, determine if the linked list has a cycle in it.

### Approach 1 (Hashing):

- Traverse the linked list and store each visited node in a hash set
- For each node, check if it already exists in the hash set
- If found, a cycle exists; if we reach the end (null), no cycle exists


> `Time Complexity` : O(n)
> 
> `Space Complexity` : O(n)

---

### Approach 2 : 

- Floyd's Cycle Detection Algorithm (Tortoise and Hare)
- Use two pointers: slow (tortoise) and fast (hare)
- Slow pointer moves one step at a time
- Fast pointer moves two steps at a time
- If there's a cycle, the fast pointer will eventually catch up to the slow pointer
- If there's no cycle, the fast pointer will reach the end (null)

[Proof](https://github.com/aagamjain04/Striver-DSA-191/blob/main/Arrays%20Part%202/Find%20the%20Duplicate%20Number.md#approach-3)

``` cpp
bool hasCycle(ListNode *head) {

	if(head==NULL)
	return false;
	ListNode* slow = head;
	ListNode* fast = head->next;

	while(fast && fast->next){
		if(slow==fast)
		return true;
		slow = slow->next;
		fast = fast->next->next;
	}

	return false;

}
```



> `Time Complexity` : O(n)
> 
> `Space Complexity` : O(1)


---

