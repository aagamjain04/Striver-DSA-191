[Problem Link](https://leetcode.com/problems/palindrome-linked-list/description/)

### Problem Statement : 

Given the `head` of a singly linked list, return `true` if it is a palindrome or `false` otherwise.

### Approach 1 (Brute Force):
- Traverse the linked list and store all values in an array/list
- Use two pointers to check if the array is a palindrome

> `Time Complexity` : O(n)
> 
> `Space Complexity` : O(n)

---

### Approach 2 (Optimal):

- Find middle using Floyd's algorithm.
- Reverse second half.
- Compare first half with reversed second half.

```cpp
ListNode* reverse(ListNode* temp){
	ListNode* curr = temp;
	ListNode* prev = NULL;

	while(curr){
		ListNode* next = curr->next;
		curr->next = prev;
		prev = curr;
		curr = next;
	}
	return prev;
}

bool isPalindrome(ListNode* head) {
	
	if(head->next==NULL){
		return true;
	}

	ListNode* slow = head;
	ListNode* fast = head->next;

	while(fast && fast->next){
		slow = slow->next;
		fast = fast->next->next;
	}

	

	ListNode* h1 = reverse(slow->next);
	ListNode* h2 = head;

	while(h1 && h2){
		if(h1->val != h2->val)
		return false;

		h1 = h1->next;
		h2 = h2->next;
	}
	return true;
}
```


> `Time Complexity` : O(n)
> 
> `Space Complexity` : O(1)

---

