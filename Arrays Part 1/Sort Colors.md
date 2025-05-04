Given an array `nums` with `n` objects colored red, white, or blue, sort them in-place so that objects of the same color are adjacent, with the colors in the order red, white, and blue.  
\
We will use the integers `0`, `1`, and `2` to represent the color red, white, and blue, respectively.  
\
You must solve this problem without using the library's sort function.  

### Approach 1 :

Just sort the array using sorting algo

> `Time Complexity` : O(nlogn)

---

### Approach 2 : 

Double pass, in first iteration store frequency and in second iteration place the values from the frequency count.

> `Time Complexity` : O(n)
> 
> `Space Complexity` : O(1)

---

### Approach 3 :

### Dutch Flag Algorithm (Single Pass Approach)

## Problem

Sort an array that contains only values 0, 1, and 2.

## Example

```
Let's say: arr = [0, 1, 2, 0, 1, 2]
```

## Algorithm

We will have 3 pointers: `l`, `m`, and `h`

```
       l              h
arr = [0, 1, 2, 0, 1, 2]
       m
```

### Logic:

```cpp
while (m <= h) {
    if (arr[m] == 0) {
        swap(arr[l], arr[m]);
        l++;
        m++;
    }
    else if (arr[m] == 2) {
        swap(arr[m], arr[h]);
        h--;
    }
    else { // arr[m] == 1
        m++;
    }
}
```




> `Time Complexity` : O(n)
> 
> `Space Complexity` : O(1)