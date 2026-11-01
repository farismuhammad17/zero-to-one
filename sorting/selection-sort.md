*This is a series of posts I hope to dedicate myself to. The topics will move through the content of **"Introduction to Algorithms, Fourth Edition" (CLRS)**, without the heavy math.*

* [Problem Statement](#problem-statement)
* [Real-life Example](#real-life-example)
* [Visualisation](#visualisation)
* [Code](#code)
* [Time Complexity](#time-complexity)
* [Memory Complexity](#memory-complexity)
* [Proof](#proof)

# Problem Statement

You have a set of numbers, you have to put it in ascending (or descending) order. Formally, it is defined as:

$Given\ a\ set\ A = \langle a_1, a_2, ..., a_n \rangle,\\ produce\ A'=\langle a_1', a_2', ..., a_n'\rangle,\\ such\ that, a_1' \le a_2' \le ... \le a_n'$

Note that the above is for ascending order, descending is flipping the sign on the last line the other way.

# Real-life Example

Imagine you have a messy row of books on a table, each with a different number on the spine, and you want to arrange them in ascending order from left to right. You decide to sort them using selection sort:

* **The Target Spot:** Look at the very first empty slot on the far left. This is where your absolute smallest number needs to end up.
* **The Full Scan:** Instead of just looking at the next item, you scan your eyes across every single remaining book on the table to hunt down the true minimum number. You keep track of the smallest one you see as you walk down the line.
* **The Swap:** Once you've looked at everything and found the absolute smallest book, you pick it up and swap its place with whatever book was currently sitting at your target spot on the far left. That first spot is now permanently sorted!
* **Repeat:** Shift your target spot one position to the right, scan all the remaining unsorted books to find the next minimum, and repeat the swap until every book is in its correct place.

# Visualisation

Selection sort, when applied to the list `[1, 3, 2, 7, 8, 4, 4, 9, 2]`, works as follows:

```
// Initial Array: [1, 3, 2, 7, 8, 4, 4, 9, 2]

// i = 0. Min from index 0..8 is 1 (index 0). Swap with index 0 (self-swap).
[[1], 3, 2, 7, 8, 4, 4, 9, 2]

// i = 1. Min from index 1..8 is 2 (index 2 or 8, pick 2). Swap index 1 (3) and index 2 (2).
[1, [2], 3, 7, 8, 4, 4, 9, 2]

// i = 2. Min from index 2..8 is 2 (index 8). Swap index 2 (3) and index 8 (2).
[1, 2, [2], 7, 8, 4, 4, 9, 3]

// i = 3. Min from index 3..8 is 3 (index 8). Swap index 3 (7) and index 8 (3).
[1, 2, 2, [3], 8, 4, 4, 9, 7]

// i = 4. Min from index 4..8 is 4 (index 5). Swap index 4 (8) and index 5 (4).
[1, 2, 2, 3, [4], 8, 4, 9, 7]

// i = 5. Min from index 5..8 is 4 (index 6). Swap index 5 (8) and index 6 (4).
[1, 2, 2, 3, 4, [4], 8, 9, 7]

// i = 6. Min from index 6..8 is 7 (index 8). Swap index 6 (8) and index 8 (7).
[1, 2, 2, 3, 4, 4, [7], 9, 8]

// i = 7. Min from index 7..8 is 8 (index 8). Swap index 7 (9) and index 8 (8).
[1, 2, 2, 3, 4, 4, 7, [8], 9]

// Final Array: [1, 2, 2, 3, 4, 4, 7, 8, 9]
```

# Code

```py
def selection_sort(arr):
    n = len(arr)
    # Move the boundary of the unsorted subarray one by one
    for i in range(n):
        # Assume the first element of the unsorted portion is the minimum
        min_idx = i

        # Find the minimum element in the remaining unsorted array
        for j in range(i + 1, n):
            if arr[j] < arr[min_idx]:
                min_idx = j

        # Swap the found minimum element with the first element of the unsorted portion
        if min_idx != i:
            arr[i], arr[min_idx] = arr[min_idx], arr[i]

    return arr
```
```c
void selection_sort(int arr[], int n) {
    // Move the boundary of the unsorted subarray one by one
    for (int i = 0; i < n - 1; i++) {
        // Assume the first element of the unsorted portion is the minimum
        int min_idx = i;

        // Find the minimum element in the remaining unsorted array
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[min_idx]) {
                min_idx = j;
            }
        }

        // Swap the found minimum element with the first element of the unsorted portion
        if (min_idx != i) {
            int temp = arr[i];
            arr[i] = arr[min_idx];
            arr[min_idx] = temp;
        }
    }
}
```

In the implementation, we move through the array from the first index (0) till the end. As we move through, we take the current index, `i`, and move through to the end to find the minimum value in `arr[i+1:]`. Let the index of the minimum of the subarray be `min_idx`.

With the minimum, we swap the current position we are on the array with the minimum, and step `i` forward.

Notice how the array `arr[:i]` is always sorted.

# Time Complexity

We iterate over the array once with `i`, then we iterate again from `i + 1` to `n` (array length). Since it is a 2D for-loop, the time complexity of selection sort is $O(n ^ 2)$.

Notice that Insertion Sort also has a time complexity of $O(n ^ 2)$, but has a best case of $O(n)$ if the array is already sorted. That is not the case with selection sort, since we have to always iterate through the array to check if the current value is actually the minimum.

# Memory Complexity

Selection sort works in place, and does not allocate or free up any portion of memory. Hence, it has a memory complexity of $O(1)$.

# Proof

As mentioned earlier, the subarray `arr[:i]` is always sorted. We know for a fact that `i` will terminate at the last value of the array. This means that the last value of `i` will be `n`.

Hence, when `i` terminates, the array `arr[0:n]` will be correctly sorted. Since `n` is the length of the array, this implies the entire array `arr` is sorted upon termination.

---

*Read on [LeetCode](https://leetcode.com/discuss/post/8597621/0-to-1-selection-sort-by-farismuhammad-ug50/).*
