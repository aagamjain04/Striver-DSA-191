[Problem Link](https://leetcode.com/problems/merge-k-sorted-lists/description/)
### Problem Statement : 

You are given an array of `k` linked-lists `lists`, each linked-list is sorted in ascending order.

Merge all the linked-lists into one sorted linked-list and return it.
## Examples

**Example 1:**

```
Input: lists = [[1,4,5],[1,3,4],[2,6]]
Output: [1,1,2,3,4,4,5,6]
Explanation: The linked-lists are:
[
  1->4->5,
  1->3->4,
  2->6
]
merging them into one sorted linked list:
1->1->2->3->4->4->5->6
```

### Approach 1 :

- Brute force
- Collect all nodes into an array.
- Sort the array.
- Build a linked list.


> `Time Complexity` : O(N log N)
> 	
> `Space Complexity` : O(N)

---

### Approach 2 :

- Divide and Conquer (Merge Sort Style)
- Merge two lists at a time.
- Merge results in pairs until one list remains.

- Start with **k** lists.
- Merge them in pairs → lists halve each round.
- Rounds needed = **log₂ k**.


> `Time Complexity` : O(N log k)
> 	
> `Space Complexity` : O(1)

---

### Approach 3 :

- Min heap approach
- Create a min heap storing `(node value, node pointer)`.
- Insert first node of each non-empty list into heap.
-  Create a dummy head for result list.
-  While heap not empty:
	   - Pop smallest node.
	   - Append to result list.
	   - Push next node from popped node's list into heap.
-  Return merged list starting at `dummy->next`.

#### Code :

```cpp
ListNode* mergeKLists(vector<ListNode*>& lists) {
	
	priority_queue<pair<int,ListNode*>,vector<pair<int,ListNode*>>,greater<pair<int,ListNode*>>> minHeap;
	ListNode* dummy = new ListNode(0);
	ListNode* head = dummy;
	int k = lists.size();
	for(int i=0;i<k;i++){

		if(lists[i]){
			minHeap.push({lists[i]->val,lists[i]});
		}
	}

	while(!minHeap.empty()){
		int val = minHeap.top().first;
		ListNode* curr = minHeap.top().second;
		minHeap.pop();

		if(curr->next){
			minHeap.push({curr->next->val,curr->next});
		}

		head->next = curr;
		head = head->next;
	}

	return dummy->next;

}

```

> `Time Complexity` : O(N log k)
> 	
> `Space Complexity` : O(k)

---
