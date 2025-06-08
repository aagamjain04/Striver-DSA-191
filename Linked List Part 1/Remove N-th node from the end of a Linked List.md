[Problem Link](https://leetcode.com/problems/remove-nth-node-from-end-of-list/description/)

### Problem Statement : 

Given the `head` of a linked list, remove the `nth` node from the end of the list and return its head.
### Approach 1 (Two pass solution):

- First pass: Calculate the total length of the linked list
- Second pass: Navigate to the (L-n)th node from the beginning
- Remove the target node by adjusting pointers

**Code** : 

``` cpp
ListNode* removeNthFromEnd(ListNode* head, int n) {
        
	ListNode* dummyNode = new ListNode(0);
	dummyNode->next= head;
	ListNode* temp = dummyNode;

	int L = 0;
	while(temp){
		L++;
		temp = temp->next;
	}

	temp = dummyNode;

	for(int i=0;i<(L-n-1);i++){
		temp = temp->next;
	}

	if(temp->next)
	temp->next = temp->next->next;

	return dummyNode->next;
}
```


> `Time Complexity` : O(L) where L is the length of linked list
> 
> `Space Complexity` :  O(1)

---

### Approach 2 (One pass solution)

- Use two pointers: `fast` and `slow`
- Move `fast` pointer n steps ahead
- Move both pointers until `fast` reaches the end
- `slow` will be at the node before the target node
- Remove the target node

Note : Create dummy node to handle edge cases

``` cpp

ListNode* removeNthFromEnd(ListNode* head, int n) {
        
	ListNode* dummyNode = new ListNode(0);
	dummyNode->next= head;
	ListNode* fast = dummyNode;
	ListNode* slow = dummyNode;

	for(int i=0;i<n;i++)
	fast = fast->next;

	while(fast->next){
		slow = slow->next;
		fast = fast->next;
	}
	if(slow->next)
	slow->next = slow->next->next;

	return dummyNode->next;
}
```



> `Time Complexity` : O(L) where L is the length of linked list
> 
> `Space Complexity` :  O(1)


---
