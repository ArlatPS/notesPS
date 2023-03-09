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
- great for mostly sorted lists that need a couple changes - best case O(n)
- overall O(n2)
- spatial O(1)
- can be stable
- destructive
- cool idea is to first try with Insertion Sort and if to many cycles then QuickSort

### Merge Sort

- divide into small arrays (length 1 at deepest recursion) and then merge (`arr1[0] > arr2[0]`) - small arrays are sorted already
- time O(log n \* n) - always the same (no best case)
- space O(n) - array for every element
- .sort() calls mergeSort and sometimes quickSort

### Quick Sort

- last element of array is the pivot, all smaller than it go to left array, bigger to right array, then `[...left, pivot, ...right]`
- worst case O(n2) - sorted or reverse sorted
- average O(n log n)
- destructive spatial O(1)
- non destructive spatial O(n) - easier
- pivot is a transitive property - if arrays are divided by pivot every element in left must be smaller then every element in right and these comparisons don't have to be made
- QuickSort 3 with pivot as a middle value of last three elements

## Binary Search

- for sorted arrays
- check middle if smaller go left, bigger go right
- time O(log n)
- can be done recursively or with a loop

## Lists

### Array List

- normal array in JS
- great for lookup O(1)
- bad for deleting (if not pop) O(n)

### Linked List

- every node has value and pointer to next node
- better for manipulation
- worse for lookup - has to go through nodes

## Binary Trees

- input must be sorted
- node has up to 2 children (left and right)
- everywhere left is smaller than node and right is bigger
- lookup O(log n) - really fast for even enormous data sets

## Bloom filters

- bloom filter is a data structure which is similar to set
- it responds to question: have you seen this before? with no (definitely) or maybe
- trading accuracy (false positives) for much smaller space complexity
- hashing functions ex. 3 that convert to ex 3 numbers
- flipping 0 -> 1 in array on according position
- during checking if at least one is 0 then it definitely wasn't previously seen
- if all 1 then maybe it was seen
