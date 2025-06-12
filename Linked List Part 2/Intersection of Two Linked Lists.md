[Problem Link](https://leetcode.com/problems/intersection-of-two-linked-lists/description/)

### Problem Statement : 

Given the heads of two singly linked-lists **headA** and **headB**, return **the node at which the two lists intersect**. If the two linked lists have no intersection at all, return **null**.

### Approach 1 (Brute Force):
- Nested loop
- Use two nested loops to compare each node of one list with every node of the other list
- For each node in the first list, traverse the entire second list to find a match
- Return the first common node found.

> `Time Complexity` : O(m * n)
> 
> `Space Complexity` : O(1)

---

### Approach 2 (Hashing):

- Traverse the first linked list and store all nodes in a hash set
- Traverse the second linked list and check if any node exists in the hash set
- Return the first node found in the hash set

```cpp
ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
    
	unordered_map<ListNode*,int> mp;
	while(headA){
		mp[headA] = 1;
		headA = headA->next;
	}

	while(headB){
		if(mp.find(headB)!=mp.end()){
			return headB;
		}
		headB = headB->next;
	}

	return NULL;
}

```


> `Time Complexity` : O(m * n)
> 
> `Space Complexity` : O(m)

---

### Approach 3 : 

- Length difference method
- Calculate the length of both linked lists
- Find the difference in lengths
- Move the pointer of the longer list forward by the difference
- Now both pointers are equidistant from the intersection point
- Move both pointers simultaneously until they meet

```cpp

ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        
	ListNode* a = headA;
	ListNode* b = headB;

	int len1 = 0, len2 = 0;
	while(a){
		a = a->next;
		len1++;
	}
	while(b){
		b = b->next;
		len2++;
	}

	if(len2>len1){
		swap(headA,headB); //let headA always point to longer list
	}

	int diff = abs(len1-len2);

	for(int i=0;i<diff;i++){
		headA = headA->next;
	}

	while(headA && headB){
		if(headA==headB)
		return headA;

		headA = headA->next;
		headB = headB->next;
	}

	return NULL;


}
```


> `Time Complexity` : O(m + n)
> 
> `Space Complexity` : O(1)

---
### Approach 4 :

- Floyd's Algorithm Variation
- Initialize two pointers at the heads of both lists
- Traverse both lists simultaneously
- When a pointer reaches the end of its list, redirect it to the head of the other list
- Continue until both pointers meet
- The meeting point is the intersection (or null if no intersection exists)

``` cpp
ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        
	ListNode* a = headA;
	ListNode* b = headB;

	while(a!=b){
		a = (a ? a->next : headB);
		b = (b ? b->next : headA);
	}

	return a;
}

```

> `Time Complexity` : O(m + n)
> 
> `Space Complexity` : O(1)

---

