[Problem Link](https://leetcode.com/problems/rotate-list/description/)
### Problem Statement : 

Given the `head` of a linked list, rotate the list to the right by `k` places.

## Examples

**Example 1:**

```
Input: head = [1,2,3,4,5], k = 2
Output: [4,5,1,2,3]
```

### Approach 1 :

- Gap method
- Find the length of the list
- Calculate effective rotation: `k = k % length`
- Now we need two pointers a and b where b points to `(l-k-1)th` node and a points to last node
- Place pointer `a` at `k` steps ahead of pointer `b`
- Move both pointers until `a` reaches the last node
- Now `b` points to the new tail, `b->next` is the new head
- Connect the old tail (`a`) to the old head
- Set new head and break the connection at new tail

```
.                new_head
                   ↓
[1] → [2] → [3] → [4] → [5] -> NULL
 ↑           ↑           ↑ 
head         b           a

break link between 3 and 4
make 4 as head
make a point to old head
		
```

#### Code :

``` cpp

ListNode* rotateRight(ListNode* head, int k) {
	if(head == NULL) return NULL;
	
	// Find the length of the list
	ListNode* temp = head;
	int l = 0;
	while(temp) {
		temp = temp->next;
		l++;
	}
	
	// Handle cases where k >= length
	k = k % l;
	
	// Two pointers: a and b
	ListNode* a = head;
	ListNode* b = head;
	
	// Move pointer 'a' k steps ahead
	int move = 0;
	while(move < k) {
		a = a->next;
		move++;
	}
	
	// Move both pointers until 'a' reaches the last node
	while(a->next) {
		a = a->next;
		b = b->next;
	}
	
	// Now 'b' points to the new tail
	// 'a' points to the current tail
	// 'b->next' will be the new head
	
	a->next = head;        // Connect tail to original head
	head = b->next;        // New head is next of new tail
	b->next = NULL;        // Break the connection
	
	return head;
}
```


> `Time Complexity` : O(n)
> 
> `Space Complexity` : O(1)
---

