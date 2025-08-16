An `array` is a contiguous collection of items, typically of the same data type. Arrays have a **predefined size** and each item has a unique index position, usually starting from 0.

```csharp
int[] myArr = new int[5];
myArr[0] = 100;
myArr[1] = 200;
myArray[2] = 300
// ...

int[] myArr2 = [100, 200, 300, 400, 500];
```

## Memory

Arrays are stored in memory as a **contiguous block**, meaning all items are right next to each other in sequential memory locations. Since memory is allocated when an array is defined, it has a set size. The array items are stored as **consecutive bytes**, meaning each item has a positional relationship with each other (0 - 1 - 2 - 3 ... n).

Items cannot be appended to an array because **something might be stored next to that array in memory**, thus effectively "blocking" that array from expanding. To effectively add a new item to an array, you would need to **create a new array instead.**

```csharp
int[] myArr = [100, 200, 300, 400, 500];

// I want to add 600 and 700

var myArr2 = new int[myArr.Length + 2];
Array.Copy(myArr, myArr2, myArr.Length);

myArr2[6] = 600;
myArr2[7] = 700;
```




