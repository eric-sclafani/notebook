Sorting algorithms arrange items in an **array**, typically in ascending order (this depends on what you're sorting, but typically its numbers).

Sorting is important because:

- Many data operations (search, merge, deduplication) are faster and more efficient once the data is sorted

- Seeing data sorted lets you spot patterns easier since data points will hold relationships.

- Different sorting algorithms have different tradeoffs and are designed for different scenarios ([[Algorithmic Complexity|time and space complexity]])

## Bubble sort

With `bubble sort`, you compare each item at index _i_ with _i+1_ and swap their positions if the _i_ item is larger than _i+1_. 

The algorithm runs **until it runs out of swaps to make**. In this way, the items "bubble" up into place, with greater values being towards the end of the array.

Bubble sort has a time complexity of ==O(n²)== because in the worst case scenario, if all elements are not sorted, you have to loop over the array **for each item in the array**. The **best case** scenario would be if the array is already most sorted, which would be a ==O(n)== runtime. 

```cs
public static void BubbleSort(int[] arr)  
{  
    var length = arr.Length - 1;  
    var madeSwap = false;  
    for (var i = 0; i < length; i++){
        for (var j = 0; j < length; j++){
            if (arr[j] > arr[j + 1]){
                (arr[j], arr[j + 1]) = (arr[j + 1], arr[j]);  
	             madeSwap = true;  
	          }  
        if (!madeSwap){
            break;  
          }       
        }    
    }
}
```


## Selection sort

With `selection sort`, each item is compared to every other item in the array for each iteration. 

For each pass of _i_, we iterate over each element that comes after _i_. After the inner loop, we swap the element at _i_ with the smallest value found.

This algorithm is always ==O(n^2)== time complexity because it iterates over the whole unsorted array for each item. 

>[!info]
>This contrasts with bubble sort because bubble sort can stop short if any iteration does not make a swap, whereas selection sort does not.

```cs
public static void SelectionSort(int[] arr)  
{  
    for (var i = 0; i < arr.Length - 1; i++){   
	    var min = i;  
        for (var j = i+1; j < arr.Length; j++){
            if (arr[j] < arr[min]){
                min = j;  
          }       
        }   
        (arr[i], arr[min]) = (arr[min], arr[i]);  
    }}
```

## Insertion Sort

`Insertion sort` works by dividing the array into **sorted** and **unsorted** portions. Each iteration over the array moves the elements from the unsorted portion to the sorted one in order. 

The sorted portion starts at the 0th index, and for each iteration, we move the current value to the sorted portion and check where it should fall into place. 

The worst case scenario is ==O(n^2).== It is suited for smaller datasets.

```cs
public static void InsertionSort(int[] arr)  
{  
    for (var i = 1; i < arr.Length; i++){  
       var value = arr[i];  
       var idx = i;  
  
       while (idx > 0 && arr[idx - 1] > value){
            arr[idx] = arr[idx - 1];  
	        idx--;  
       }       
       arr[idx] = value;  
    }
}
```



## Merge sort

`Merge sort` is a **divide-and-conquer** algorithm that splits the input array, sorts the individual pieces, and then recombines them. This is done using [[Recursion|recursion]].

 The primary idea is to continuously **divide** the array into smaller parts. After this splitting, the smaller arrays are sorted one by one and then the elements are merged back together (**conquer**). 

In the worst case, merge sort has a time complexity of ==O(n log n)==, making it much faster than bubble, selection, and insertion sort.

One trade-off to consider is that the **space complexity** is ==O(n)== because a new subarray is created for each item.

```cs
public static void MergeSort(int[] arr)  
{  
    var n = arr.Length;  
    var mid = n / 2;  
    var left = arr[..mid];  
    var right = arr[mid..];  
    
    if (n < 2) return; // base case: arr has 1 element  
    
    MergeSort(left);  
    MergeSort(right);  
    Merge(arr, left, right);  
}  
  
private static void Merge(int[] arr, int[] left, int[] right)  
{  
    int i = 0, j = 0, k = 0;  
  
    while (i < left.Length && j < right.Length){
	    if (left[i] <= right[j]){
		    arr[k++] = left[i++];  
	    }  
        else {
	        arr[k++] = right[j++];  
         }   
    }  
    
    // copy remaining leftovers from left
    while (i < left.Length){    
	    arr[k++] = left[i++];  
	}    
	
	// copy remaining leftovers from right
	while (j < right.Length){
		arr[k++] = right[j++];  
    }    
}
```
