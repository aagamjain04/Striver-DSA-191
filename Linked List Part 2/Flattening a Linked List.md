[Problem Link](https://www.geeksforgeeks.org/problems/flattening-a-linked-list/1)
### Problem Statement : 

Given a linked list containing ‘N’ head nodes where every node in the linked list contains two pointers:

1. ‘Next’ points to the next node in the list
2. ‘Child’ pointer to a linked list where the current node is the head

Each of these child linked lists is in sorted order and connected by a 'child' pointer. Your task is to flatten this linked list such that all nodes appear in a single layer or level in a 'sorted order'.

### Approach 1 (Brute force):

- Iterate over entire list and store the value in array
- Sort the array
- Transform array into list

> `Time Complexity` : O(NM) + O(NM log(NM)) + O(NM) where N is the length of the linked list along the next pointer and M is the length of the linked list along the child pointer.

1. O(NM) as we traverse through all the elements, iterating through ‘N’ nodes along the next pointer and ‘M’ nodes along the child pointer.
2. O(NM log(NM)) as we sort the array containing NM (total) elements.
3. O(NM) as we reconstruct the linked list from the sorted array by iterating over the NM elements of the array.
> 
> `Space Complexity` : O(NM) + O(NM) here N is the length of the linked list along the next pointer and M is the length of the linked list along the child pointer.

4. O(NM) for storing all the elements in an additional array for sorting.
5. O(NM) to reconstruct the linked list from the array after sorting

---

### Approach 2 : 

- Merge K sorted lists
- Each node's `bottom` chain is already sorted
- We process from right to left (due to recursion)
- Merge two sorted lists at each step

``` cpp
// The flattened list will be printed using the **bottom** pointer instead of the next pointer.

Node* merge(Node* l1,Node* l2){
        
	Node* dummy = new Node(0);
	Node* curr = dummy;
	
	while(l1 && l2)
	{
		if(l1->data <= l2->data){
			curr->bottom = l1;
			l1 = l1->bottom;
		}else{
			curr->bottom = l2;
			l2 = l2->bottom;
		}
		curr = curr->bottom;
	}
	
	if(l1)
	curr->bottom = l1;
	
	if(l2)
	curr->bottom = l2;
	
	return dummy->bottom;
}


// Function which returns the  root of the flattened linked list.
Node *flatten(Node *root) {
	
	if(root->next==NULL){
		return root;
	}
	
	Node* next = flatten(root->next);
	
	return merge(root,next);
	
}

```



> `Time Complexity` : O(NM) where N is the length of the linked list along the next pointer and M is the length of the linked list along the child pointer.
> 
> `Space Complexity` : O(H)where H is the height of recursion (number of nodes in horizontal direction)


---

