[Problem Link](https://leetcode.com/problems/find-median-from-data-stream/description/)
### Problem Statement : 

Implement the MedianFinder class:

- `MedianFinder()` initializes the `MedianFinder` object.
- `void addNum(int num)` adds the integer `num` from the data stream to the data structure.
- `double findMedian()` returns the median of all elements so far. Answers within `10-5` of the actual answer will be accepted.
## Examples

**Example 1:**

```
Input
["MedianFinder", "addNum", "addNum", "findMedian", "addNum", "findMedian"]
[[], [1], [2], [], [3], []]
Output
[null, null, null, 1.5, null, 2.0]

Explanation
MedianFinder medianFinder = new MedianFinder();
medianFinder.addNum(1);    // arr = [1]
medianFinder.addNum(2);    // arr = [1, 2]
medianFinder.findMedian(); // return 1.5 (i.e., (1 + 2) / 2)
medianFinder.addNum(3);    // arr[1, 2, 3]
medianFinder.findMedian(); // return 2.0
```

### Approach 1 :

- Use **two heaps**:
	1. **Max Heap** (`left`) → stores smaller half
	2. **Min Heap** (`right`) → stores larger half
- If `num <= left.top()` → push to `left`
- Else → push to `right`
- Balance:
	   - If `left.size() > right.size() + 1` → move top of `left` to `right`
	   - If `right.size() > left.size()` → move top of `right` to `left`


## Complexity
- **addNum**: O(log n)
- **findMedian**: O(1)
- Space: O(n)

#### Code : 

```cpp
class MedianFinder {
public:

    priority_queue<int> left;
    priority_queue<int,vector<int>,greater<int>> right;


    MedianFinder() {
        
    }
    
    void addNum(int num) {

        if(left.empty()){
            left.push(num);
        }else if(left.size() == right.size()){

            if(num > right.top()){
                left.push(right.top());
                right.pop();
                right.push(num);
            }else{
                left.push(num);
            }

        }else{

            if(num>=left.top()){
                right.push(num);
            }else{
                right.push(left.top());
                left.pop();
                left.push(num);
            }
        }
        
    }
    
    double findMedian() {
        if(left.size()==right.size()){
            return double(left.top() + right.top())/2.0;
        }else{
            return double(left.top());
        }
    }
};

```

---

**Follow up:**

- If all integer numbers from the stream are in the range `[0, 100]`, how would you optimize your solution?
	- Maintain a frequency array and count
- If `99%` of all integer numbers from the stream are in the range `[0, 100]`, how would you optimize your solution?
	- Maintain a frequency array and count for number less than 100 and heaps for greater numbers.

---

