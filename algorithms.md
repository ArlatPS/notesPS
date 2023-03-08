# Data Structures and Algorithms

- recursive functions often
  - if statement to leave with return;
  - return with call for itself

## Big O

- time complexity - how much computations/time will algorithm take
- spatial complexity - how much memory (mainly RAM) will computation take
- names
  - O(n2) - quadratic time
  - O(log n \* n)
  - O(n) - linear time
  - O(1) - constant time

## Sorts

### Bubble Sort

- swapping 2 (higher to the right) - big values bubble to the top
- time O(n2)
- after first iteration last pair doesn’t have to be checked and so forth (optimization)
- stable sorting algorithm - if 2 items equal first in original will be first in output
- destructive - changes original array

### Insertion Sort

- takes unsorted (smaller than the one before) and inserts in the right place (lower in the array)
- great for mostly sorted lists that nedd a couple changes - best case O(n)
- overall O(n2)
- spatial O(1)
- can be stable
- destructive
- cool idea is to first try with Insertion Sort and if to many cycles then QuickSort

### Merge Sort

- divide into small arrays (lenght 1 at deepest recursion) and then merge (`arr1[0] > arr2[0]`) - small arrays are sorted already
- time O(log n \* n) - always the same (no best case)
- space O(n) - array for every element
- .sort() calls mergeSort and sometimes quickSort

### Quick Sort

- last element of array is the pivot, all smaller than it go to left array, bigget to right array, then `[...left, pivot, ...right]`
- worst case O(n2) - sorted or reverse sorted
- average O(n log n)
- destructive spatial O(1)
- non destructive spatial O(n) - easier
- pivot is a transitive property - if arrays are divided by pivot every element in left must be smaller then every element in rigth and these comparisons don't have to be made
- QuickSort 3 with pivot as a middle value of last three elements

## Binary Tree
