[Problem Link](https://leetcode.com/problems/merge-intervals/description/)

## Problem Statement

Given an array of `intervals` where `intervals[i] = [starti, endi]`, merge all overlapping intervals, and return _an array of the non-overlapping intervals that cover all the intervals in the input_.

## Examples

**Example 1:**

```
Input: intervals = [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlap, merge them into [1,6].
```

**Example 2:**

```
Input: intervals = [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.
```

## Approach

1. **Sort the intervals** based on the start time.
2. **Initialize** a result array with the first interval.
3. **Iterate** through the intervals:
    - If the current interval overlaps with the last interval in the result, merge them.
    - Otherwise, add the current interval to the result.
4. **Return** the result array.

## How to determine if intervals overlap?

Two intervals `[a, b]` and `[c, d]` overlap if:

- `c <= b` (the start of the second interval is less than or equal to the end of the first)

## How to merge overlapping intervals?

When merging two intervals `[a, b]` and `[c, d]`:

- The start of the merged interval is `min(a, c)`
- The end of the merged interval is `max(b, d)`

## Algorithm

```cpp
vector<vector<int>> merge(vector<vector<int>>& intervals) {
    if (intervals.empty()) return {};
    
    // Sort intervals based on start time
    sort(intervals.begin(), intervals.end());
    
    vector<vector<int>> result;
    result.push_back(intervals[0]);
    
    for (int i = 1; i < intervals.size(); i++) {
        // Get the last interval in the result
        vector<int>& lastInterval = result.back();
        int currentStart = intervals[i][0];
        int currentEnd = intervals[i][1];
        
        // If current interval overlaps with last interval in result
        if (currentStart <= lastInterval[1]) {
            // Merge the intervals
            lastInterval[1] = max(lastInterval[1], currentEnd);
        } else {
            // Add current interval to result
            result.push_back(intervals[i]);
        }
    }
    
    return result;
}
```

## Time and Space Complexity

- **Time Complexity**: O(n log n), where n is the number of intervals.
    
    - Sorting takes O(n log n) time
    - Merging takes O(n) time
- **Space Complexity**: O(n) for storing the result. If we don't count the output array, the space complexity would be O(1).
    


## Visual Example

For input `[[1,3],[2,6],[8,10],[15,18]]`:

1. Sort (already sorted): `[[1,3],[2,6],[8,10],[15,18]]`
2. Initialize result with first interval: `[[1,3]]`
3. Process [2,6]:
    - 2 <= 3, so merge [1,3] and [2,6] to get [1,6]
    - Result: `[[1,6]]`
4. Process [8,10]:
    - 8 > 6, so add [8,10] to result
    - Result: `[[1,6],[8,10]]`
5. Process [15,18]:
    - 15 > 10, so add [15,18] to result
    - Result: `[[1,6],[8,10],[15,18]]`
6. Return `[[1,6],[8,10],[15,18]]`