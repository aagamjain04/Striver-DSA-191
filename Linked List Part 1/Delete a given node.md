[Problem Link](https://leetcode.com/problems/delete-node-in-a-linked-list/submissions/1470367051/)

### Problem Statement : 

Write a function to **delete a node** in a singly-linked list. You will **not** be given access to the head of the list instead, you will be given access to **the node to be deleted** directly. It is **guaranteed**
that the node to be deleted is **not a tail node** in the list.

### Approach 1:

- Copy the data from the successor node into the current node to be deleted.
- Update the `next` pointer of the current node to reference the `next` pointer of the successor node.

**Code** :

``` cpp
  void deleteNode(ListNode* node) {
        
	node->val = node->next->val;
	ListNode* temp = node->next;
	node->next = node->next->next;
	delete temp;

}
```


> `Time Complexity` : O(1)
> 
> `Space Complexity` : O(n)

---
