[Problem Link](https://leetcode.com/problems/trapping-rain-water/description/)
### Problem Statement : 

You are given timings of **n** meetings in the form of `(start[i], end[i])` where `start[i]` is the start time of meeting `i` and `end[i]` is the finish time of meeting **i.** Return the **maximum** number of meetings that can be accommodated in a single meeting room, when only one meeting can be held in the meeting room at a particular time. 

**Note:** The start time of one chosen meeting can't be equal to the end time of the other chosen meeting.

## Examples

**Example 1:**

```
Input: start[] = [1, 3, 0, 5, 8, 5], end[] =  [2, 4, 6, 7, 9, 9]
Output: 4
Explanation: Maximum four meetings can be held with given start and end timings. The meetings are - (1, 2), (3, 4), (5,7) and (8,9)
```

### Approach 1 :

- Greedy approach
- To **maximize the number of non-overlapping meetings**, always **choose the meeting that finishes the earliest** (i.e., with the **smallest end time**).
- Store all meetings as pairs `(start[i], end[i])`.
- Sort the meetings based on their **end time**.
- Initialize a variable `last_end` to keep track of the end time of the last selected meeting.
- Traverse through sorted meetings:
    - If `start > last_end`, select the meeting.
    - Update `last_end` to the current meeting's end time.


#### Code :

``` cpp
int maxMeetings(vector<int>& start, vector<int>& end) {
	// Your code here
	int n = start.size();
	vector<pair<int,int>> vp;
	for(int i=0;i<n;i++){
		vp.push_back({end[i],start[i]});
	}
	
	sort(vp.begin(),vp.end());
	
	int last_end = vp[0].first;
	
	int meetings = 1;
	for(int i=1;i<n;i++){
		int start = vp[i].second;
		if(start > last_end){
			meetings++;
			last_end = vp[i].first;
		}
	}
	
	return meetings;
	
}

```


> `Time Complexity` : O(nlogn)
> 
> `Space Complexity` : O(n)


---


