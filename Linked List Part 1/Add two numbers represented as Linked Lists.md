[Problem Link](https://leetcode.com/problems/add-two-numbers/description/)

### Problem Statement : 

You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order**, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

### Approach 1:

- Note that we use a dummy head to simplify the code. Without a dummy head, you would have to write extra conditional statements to initialize the head's value.

**Code** :

``` cpp
ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
	ListNode* dummyNode = new ListNode(0);
	ListNode* curr = dummyNode;
	int carry = 0;
	while(l1 || l2 || carry)
	{
		int a = l1 ? l1->val : 0;
		int b = l2 ? l2->val : 0;
		int sum = a+b+carry;
		ListNode* node = new ListNode(sum%10);
		carry = sum/10;
		curr->next = node;
		curr = node;

		l1 = l1 ? l1->next : NULL;
		l2 = l2 ? l2->next : NULL;
	}

	return dummyNode->next;
}
```


> `Time Complexity` : O(n)
> 
> `Space Complexity` : O(n)

---
