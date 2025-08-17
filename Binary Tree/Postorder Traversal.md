[Problem Link](https://leetcode.com/problems/binary-tree-postorder-traversal/description/)
### Problem Statement : 

Given the `root` of a binary tree, return _the postorder traversal of its nodes' values_.

**Example 1:**

```
Input :
    1
     \
      2
     /
    3
Output : 3 2 1


```

---

###  Approach 1 :

- Recursive approach
- Traverse left → Traverse right → Visit root.

#### Code :

```cpp
void go(TreeNode* root,vector<int> &res){
	if(root==NULL)
	return;

	go(root->left,res);
	go(root->right,res);
	res.push_back(root->val);
}

vector<int> postorderTraversal(TreeNode* root) {
	
	vector<int> res;
	go(root,res);
	return res;

}
```


> `Time Complexity` : `O(n)`
> 
> `Space Complexity` : O(h)-> (due to recursion stack, where `h` = height of tree). Worst case `O(n)` for skewed tree, `O(log n)` for balanced tree.

---

### Approach 2 :

- Iterative Two Stacks
- Use stack1 for traversal, stack2 to reverse order.
- Push root → Pop from stack1 → push children → push node into stack2.
- Finally, stack2 gives postorder.

#### Code :

```cpp
vector<int> postorderTraversal(TreeNode* root) {
	
	vector<int> res;
	stack<TreeNode*> st1,st2;
	if(root)
	st1.push(root);
	while(!st1.empty()){

		TreeNode* curr = st1.top();
		st1.pop();
		st2.push(curr);

		if(curr->left)
		st1.push(curr->left);

		if(curr->right)
		st1.push(curr->right);
	   
	}
	while(!st2.empty()){
		TreeNode* curr = st2.top();
		res.push_back(curr->val);
		st2.pop();
	}
	return res;

}
```


> `Time Complexity` : `O(n)`
> 
> `Space Complexity` : O(n)


---

### Approach 3 :

- Iterative one stack
- Do preorder traversal with one change
- Root -> Right -> Left
- Reverse the result


```
          1
        /   \
       3     2
            / \      
           4   5
                \            
                 6

Preorder(swap)[root right left] : 1 2 5 6 4 3
Reverse -> PostOrder [left right root]
3 4 6 5 2 1 
```

#### Code :

``` cpp
vector<int> postorderTraversal(TreeNode* root) {
	
	vector<int> res;
	stack<TreeNode*> st;
	if(root)
	st.push(root);
	while(!st.empty()){

		TreeNode* curr = st.top();
		st.pop();
		res.push_back(curr->val);

		if(curr->left)
		st.push(curr->left);

		if(curr->right)
		st.push(curr->right);

	}
	reverse(res.begin(),res.end());
	return res;

}
};
```


> `Time Complexity` : `O(n)`
> 
> `Space Complexity` : O(n)


---

### Approach 4 :

- Morris PostOrder
- Similar to morris Preorder but with reverse direction `root right left`
- Reverse the result

#### Code :

``` cpp
vector<int> postorderTraversal(TreeNode* root) {
	
	vector<int> res;
	
	TreeNode* curr = root;

	while(curr){

		if(curr->right == NULL){
			res.push_back(curr->val);
			curr = curr->left;
		}else{

			TreeNode* rightPart = curr->right;
			while(rightPart->left && rightPart->left!=curr){
				rightPart = rightPart->left;
			}

			if(rightPart->left == curr){
				rightPart->left = NULL;
				curr = curr->left;

			}else{
				rightPart->left = curr;
				res.push_back(curr->val);
				curr = curr->right;
			}



		}
	}

	reverse(res.begin(),res.end());
	return res;

}
```


> `Time Complexity` : `O(n)`
> 
> `Space Complexity` : O(1)

---
