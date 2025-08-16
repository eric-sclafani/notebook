Sorting algorithms arrange items in an **array**, typically in ascending order (this depends on what you're sorting, but typically its numbers).


## Bubble sort

With `bubble sort`, you compare each item at index _i_ with _i+1_ and swap their positions if the _i_ item is larger than _i+1_. 

The algorithm runs **until it runs out of swaps to make**. In this way, the items "bubble" up into place, with greater values being towards the end of the array.

Bubble sort has a time complexity of ==O(n²)== because in the worst case scenario, if all elements are not sorted, you have to loop over the array **for each item in the array**. This is a very slow run time. 

## Merge sort

