[Problem Link](https://leetcode.com/problems/linked-list-cycle-ii/description/)
### Problem Statement : 

Given the `head` of a linked list, return the node where the cycle begins. If there is no cycle, return `null`.

### Approach 1 (Hashing):

- Traverse the linked list and store each visited node in a hash set
- The first node we encounter that's already in the set is the cycle start
- If we reach null, there's no cycle

> `Time Complexity` : O(n)
> 
> `Space Complexity` : O(n)

---

### Approach 2 : 

- Use Floyd's algorithm to detect cycle.
- Both slow and fast will meet at a node.
- Keep one node at meeting point and one at starting point.
- Move one step at a time, the point at which both meet is the starting of loop.

[Proof](https://github.com/aagamjain04/Striver-DSA-191/blob/main/Arrays%20Part%202/Find%20the%20Duplicate%20Number.md#approach-3)

``` cpp
ListNode *detectCycle(ListNode *head) {
        
	if(head==NULL || head->next==NULL)
	return NULL;

	ListNode* slow = head;
	ListNode* fast = head;

	while(fast && fast->next){
		
		slow = slow->next;
		fast = fast->next->next;

		if(fast==slow)
		break;
	}

	if(fast==NULL || fast->next==NULL)
	return NULL;

	ListNode* p1 = head;
	ListNode* p2 = slow;

	while(p1!=p2){
		p1 = p1->next;
		p2 = p2->next;
	}
	return p1;
}
```



> `Time Complexity` : O(n)
> 
> `Space Complexity` : O(1)


---

