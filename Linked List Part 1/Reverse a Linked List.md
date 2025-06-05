[Problem Link](https://leetcode.com/problems/reverse-linked-list/description/)

### Problem Statement : 

Given the `head` of a singly linked list, reverse the list, and return _the reversed list_.

Note : A linked list can be reversed either iteratively or recursively.

### Approach 1 (Iterative):

```cpp

ListNode* reverseList(ListNode* head) {

   ListNode* prev = NULL;
   ListNode* curr = head;

   while(curr)
   {
		ListNode* temp = curr->next;
		curr->next = prev;
		prev = curr;
		curr = temp;
   } 
   return prev;
}
```


> `Time Complexity` : O(n)
> 
> `Space Complexity` : O(1)

---

### Approach 2 (Recursive)

``` cpp
 ListNode* reverseList(ListNode* head) {

	if(head==NULL || head->next==NULL){
		return head;
	}

	ListNode* newHead = reverseList(head->next);
	head->next->next = head;
	head->next = NULL;
	return newHead;
}
```

> `Time Complexity` : O(n)
> 
> `Space Complexity` : O(1) , though stack space will be used due to recursion

---

