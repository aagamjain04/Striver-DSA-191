[Problem Link](https://www.geeksforgeeks.org/problems/job-sequencing-problem-1587115620/1)
### Problem Statement : 

You are given two arrays: **`deadline[]`,** and **`profit[]`,** which represent a set of jobs, where each job is associated with a **deadline**, and a **profit**. Each job takes 1 unit of time to complete, and only one job can be scheduled at a time. You will earn the profit associated with a job only if it is completed by its deadline.

Your task is to find:

1. The **maximum number of jobs** that can be completed within their deadlines.
2. The **total maximum profit** earned by completing those jobs.

## Examples

**Example 1:**

```
Input: deadline[] = [4, 1, 1, 1], profit[] = [20, 10, 40, 30]
Output: [2, 60]
Explanation: Job1 and Job3 can be done with maximum profit of 60 (20+40).
```


**Example 2:**

```
Input: deadline[] = [2, 1, 2, 1, 1], profit[] = [100, 19, 27, 25, 15]
Output: [2, 127]
Explanation: Job1 and Job3 can be done with maximum profit of 127 (100+27).
```
### Approach 1 :

 - Sort jobs by profit in descending order - prioritize high-profit jobs
 - Use a time slot tracking mechanism to schedule jobs
 - For each job, find the latest available time slot before or at its deadline

#### Logic :
- Sort the jobs in descending order of profit.
- If the maximum deadline is x, make an array of size x .Each array index is set to 0 initially as no jobs have been performed yet.
- For every job check if it can be performed on its last day.
- If possible mark that index  and add the profit to our answer.
- If not possible, loop through the previous indexes until an empty slot is found.

#### Code :

``` cpp
vector<int> jobSequencing(vector<int> &deadline, vector<int> &profit) {
        
	vector<pair<int,int>> vp;
	int n = deadline.size();
	for(int i=0;i<n;i++){
		vp.push_back({profit[i],deadline[i]});
	}
	
	sort(vp.rbegin(),vp.rend());
	
	vector<int> status(n+1,0);
	
	int count = 0;
	int totalProfit = 0;
	for(auto &i:vp){
		int currDeadline = i.second;
		int currProfit = i.first;
		
		while(currDeadline>0){
			if(status[currDeadline] == 0){
				status[currDeadline] = 1;
				count++;
				totalProfit+=currProfit;
				break;
				
			}
			currDeadline--;
		}
		
	}
	
	return {count,totalProfit};
}

```


> `Time Complexity` : O(nlogn + n²)
> 
> `Space Complexity` : O(n)


---


### Approach 2 :

- Min heap strategy
- Jobs with earlier deadlines are processed first
- We can efficiently replace lower-profit jobs when better opportunities arise
- The heap size represents the number of time slots used

#### Logic :

- Create job pairs `(deadline, profit)` and sort by deadline
- **Use min-heap** to track profits of selected jobs
- For each job in deadline order:
    - If `deadline > heap_size`: We have available slots, add job
    - If `deadline ≤ heap_size` and `current_profit > min_profit_in_heap`: Replace the lowest profit job
    - Otherwise: Skip the job

#### Code :

``` cpp
vector<int> jobSequencing(vector<int> &deadline, vector<int> &profit) {
    vector<pair<int,int>> jobs;
    int n = deadline.size();
    
    // Create (deadline, profit) pairs
    for(int i = 0; i < n; i++) {
        jobs.push_back({deadline[i], profit[i]});
    }
    
    // Sort by deadline in ascending order
    sort(jobs.begin(), jobs.end());
    
    // Min-heap to store profits of selected jobs
    priority_queue<int, vector<int>, greater<int>> minHeap;
    
    int totalProfit = 0;
    
    for(auto &job : jobs) {
        int currDeadline = job.first;
        int currProfit = job.second;
        
        if(currDeadline > minHeap.size()) {
            // We have available time slots
            minHeap.push(currProfit);
            totalProfit += currProfit;
        } 
        else if(!minHeap.empty() && minHeap.top() < currProfit) {
            // Replace lowest profit job with current higher profit job
            totalProfit -= minHeap.top();
            minHeap.pop();
            minHeap.push(currProfit);
            totalProfit += currProfit;
        }
        // Otherwise skip this job
    }
    
    return {(int)minHeap.size(), totalProfit};
} 

```


> `Time Complexity` : O(nlogn) -> Sorting + Heap operation
> 
> `Space Complexity` : O(n) -> for heap

---

