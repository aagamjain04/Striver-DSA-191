[Problem Link](https://leetcode.com/problems/merge-two-sorted-lists/description/)

### Problem Statement : 

Given two sorted linked lists, merge them to produce a combined sorted linked list. Return the head of the final linked list created.
### Approach 1 (Brute Force):

- The brute force approach would be to extract all the elements from both the lists into an additional array then sorting the array and creating a new combined linked list.


> `Time Complexity` : **O(N1 + N2) + O(N log N) + O(N)** where N1 is the number of linked list nodes in the first list and N2 is the number of linked list nodes in the second list and N is the total number of nodes (N1+N2).
> 
> `Space Complexity` :  O(N)+O(N) , external array and combined list

---

### Approach 2 (Optimal)

- We can leverage the property that the given linked lists would be sorted. We employ a pointer based merging strategy where nodes from both linked lists are rearranged to form a single sorted linked list.
- Start with a dummy node for simplicity.

``` cpp

 ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
	
	ListNode* head = new ListNode(0);
	ListNode* temp = head;

	while(list1 && list2){
		if(list1->val <= list2->val){
			temp->next = list1;
			list1 = list1->next;
		}else{
			temp->next = list2;
			list2 = list2->next;
		}
		temp = temp->next;
	}

	temp->next = (list1) ? list1 : list2;

	return head->next;

}
```



> `Time Complexity` : O(N1+N2)
> 
> `Space Complexity` : O(1)


---
