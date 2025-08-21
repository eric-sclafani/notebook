Searching algorithms are essential for seeking specific items in a data structure.


## Linear search

Perhaps the easiest to implement, `linear search` is as simple as iterating through an array and if you come across the target item, you return that item's index. Otherwise, return a value indicating that the target does not exist in the array (null, -1, etc...).

The time complexity is ==O(n)== in the worst case.

```cs
public static int LinearSearch(int[] arr, int target)  
{  
    for (var i = 0; i < arr.Length; i++){
	    if (arr[i] == target){
		    return i;  
        }   
    }  
    return -1;  
}
```

# Binary search

`Binary search` follows the **divide and conquer** technique that continuously cuts the array in half while searching. The array _has_ to be **sorted** for this algorithm to work.

Binary search is not that efficient for smaller datasets, but shines when working on **larger datasets**. 

Time complexity is ==O(log n)== because at each step, we are eliminating half of the array to search through.

```cs
public static int BinarySearch(int[] arr, int target, int offset=0)  
{  
    var length = arr.Length;  
    var mid = length / 2;  
    if (arr.Length == 0) return -1; // if target not found  
  
    if (arr[mid] == target)  return offset+mid;  
    
    // offset lets us keep track of original array index positions
    if (arr[mid] > target){  
	    return BinarySearch(arr[..mid], target, offset);  
    }    
    
    if (arr[mid] < target){  
        return BinarySearch(arr[(mid+1)..], target, offset+mid+1);    
	}  
  
    return -1;  
}
```





