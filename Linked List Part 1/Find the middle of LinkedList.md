[Problem Link](https://leetcode.com/problems/middle-of-the-linked-list/description/)

### Problem Statement : 

Given the `head` of a singly linked list, return _the middle node of the linked list_.

If there are two middle nodes, return **the second middle** node.

### Approach 1 (Brute Force):

- Using the brute force approach, we can find the middle node of a linked list by traversing the linked list and finding the total number of nodes as `count`. Then we reset the traversal pointer and traverse to the node at the `[count/2 + 1]th` position. That will be the middle node.
- Double iteration.


> `Time Complexity` : O(n)
> 
> `Space Complexity` : O(1)

---

### Approach 2 (Optimal)

- Doing it in single pass using Tortoise and Hare Algorithm.

``` cpp
ListNode* middleNode(ListNode* head) {
        
	ListNode* slow = head;
	ListNode* fast = head;

	while(fast && fast->next){
		slow = slow->next;
		fast = fast->next->next;
	}

	return slow;
}


```



> `Time Complexity` : O(n)
> 
> `Space Complexity` : O(1)


---
