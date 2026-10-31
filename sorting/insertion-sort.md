*This is a series of posts I hope to dedicate myself to. This is the first of these, and the topics will move through the content of **"Introduction to Algorithms, Fourth Edition" (CLRS)**, without the heavy math.*

* [Problem Statement](#problem-statement)
* [Real-life Example](#real-life-example)
* [Visualisation](#visualisation)
* [Code](#code)
* [Time Complexity](#time-complexity)
* [Memory Complexity](#memory-complexity)
* [Proof](#proof)

# Problem Statement

Sorting. Everyone knows it. It's quite fundamental to the field - you have a set of numbers, you have to put it in ascending (or descending) order. Formally, it is defined as:

$Given\ a\ set\ A = \langle a_1, a_2, ..., a_n \rangle,\\ produce\ A'=\langle a_1', a_2', ..., a_n'\rangle,\\ such\ that, a_1' \le a_2' \le ... \le a_n'$

Note that the above is for ascending order, descending is flipping the sign on the last line the other way.

# Real-life Example

Imagine you have a messy row of papers spread out on a desk, each with a number on it. You want to sort them in ascending order. You, as a computer, decide to sort them using insertion sort:

1. **The Starting Anchor:** Look at the very first paper on the left. By itself, a single paper is already "sorted." Consider this your sorted section.
2. **Pick and Slide:** Pick up the next paper from the unsorted pile. Compare its number moving backward through your sorted papers (from right to left).
3. **Finding the Slot:** If the new paper is smaller than the one you're comparing it to, shift that paper one spot to the right. Keep moving backward until you find a paper smaller than your new one, and drop your paper right into that empty slot.
4. **Repeat:** Do this for every remaining paper until the whole desk is ordered.

# Visualisation

Insertion sort is very straightforward, so here is the list `[1, 3, 2, 7, 8, 4, 4, 9, 2]` being sorted in it, step by step.

```
// Initial Array: [1, 3, 2, 7, 8, 4, 4, 9, 2]

// i = 1, key = 3. Compare with 1. Already in order.
[1, [3], 2, 7, 8, 4, 4, 9, 2]

// i = 2, key = 2. Compare backward, shift 3 right, insert 2.
[1, [2], 3, 7, 8, 4, 4, 9, 2]

// i = 3, key = 7. Compare with 3. Already in order.
[1, 2, 3, [7], 8, 4, 4, 9, 2]

// i = 4, key = 8. Compare with 7. Already in order.
[1, 2, 3, 7, [8], 4, 4, 9, 2]

// i = 5, key = 4. Shift 8 and 7 right, insert 4.
[1, 2, 3, [4], 7, 8, 4, 9, 2]

// i = 6, key = 4. Shift 8 and 7 right, insert 4.
[1, 2, 3, 4, [4], 7, 8, 9, 2]

// i = 7, key = 9. Compare with 8. Already in order.
[1, 2, 3, 4, 4, 7, 8, [9], 2]

// i = 8, key = 2. Shift 9, 8, 7, 4, 4, 3 right, insert 2 after 1.
[1, [2], 2, 3, 4, 4, 7, 8, 9]

// Final Array: [1, 2, 2, 3, 4, 4, 7, 8, 9]
```

# Code

Hopefully by now, Insertion sort is intuitive enough and understood. So we'll try to implement it both in Python and C, which should cover the broad range of understanding.

```py
def insertion_sort(arr):
    # Start from the second element (index 1)
    for i in range(1, len(arr)):
        key = arr[i]
        j = i - 1

        # Walk backward, shifting elements to the right until
        # we find where 'key' belongs in the sorted portion
        while j >= 0 and arr[j] > key:
            arr[j + 1] = arr[j]
            j -= 1

        # Drop the key into its correct sorted slot
        arr[j + 1] = key

    return arr
```
```c
void insertion_sort(int arr[], int n) {
    // Start from the second element (index 1)
    for (int i = 1; i < n; i++) {
        int key = arr[i];
        int j = i - 1;

        // Walk backward, shifting elements to the right until
        // we find where 'key' belongs in the sorted portion
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }

        // Drop the key into its correct sorted slot
        arr[j + 1] = key;
    }
}
```

As you may see, we start by iterating through the array, skipping the first element. This is because the first element cannot move back. Note an interesting observation: if we take the array of all values before `i`, that array is, indeed, sorted. This becomes useful later.

We store the value at the current `i`, the current value will get written over by the values before it. Since we need this value for obvious reasons, we store the current value in a variable `key`.

Iterating through the array, we take the key, move all previous values forward till we find the first value not greater than the key.

Then we put the key into that newly created slot inside the array.

# Time Complexity

Often, it is more useful to know the worst case running time of a function than the best case. This is because the worst case is a better discriminator than the best case.

**Worst case**, the array is in descending order and we want to sort it into ascending. At a high level explanation, if the array has a length of `n`, we must iterate through the array, giving a factor of `n` to the time complexity. We must also move all subsequent values forward, giving another factor of `n` into the time complexity. Hence, insertion sort has a time complexity of $O(n^2)$.

**Best case** is rarely a useful descriptor for an algorithm, but in the case of insertion sort, it is when the array is already sorted. Still, we iterate through the array once, hence the best case time complexity is $O(n)$.

# Memory Complexity

Insertion sort works in place, and does not allocate or free up any portion of memory. Hence, it has a memory complexity of $O(1)$.

# Proof

It might be surprising that we need *"proof"* that insertion sort works, but what is to say that this general problem applies for every possible array of values. Proofs for an algorithm are mostly done using induction, where we assume a base case $S_0$, and prove that if $S_n$ holds, so does $S_{n+1}$. An algorithm also requires one extra condition for its proof: the fact that it terminate in finite time.

Base case of insertion sort is the fact that the first element in the array is already "sorted". Remember that all values before `i` are actually sorted if taken as a separate array? This is where it's handy! This means, if the array given was `arr`, then `arr[0:i]` is correctly sorted.

We know for a fact that `i` will terminate at the last value of the array. This means that the last value of `i` will be `n`.

Hence, when `i` terminates, the array `arr[0:n]` will be correctly sorted. Since `n` is the length of the array, this implies the entire array `arr` is sorted.

---

*Read on [LeetCode](https://leetcode.com/discuss/post/8596021/0-to-1-insertion-sort-by-farismuhammad-a9wr/)*
