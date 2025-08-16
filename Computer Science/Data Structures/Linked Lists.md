
A `Linked list` is a data structure that stores a collection of items. They are similar to arrays, but have a few key differences.

While arrays can be visualized as  a contiguous box of items of the same size,  linked lists are **many individual boxes linked to each other**.

Each item in a linked list can be represented as an object like so:

```csharp
public class Node {
	int data;
	Node next;

	public Node(int _data=0, Node _next = null){
		data = _data;
		next = _next;
	}
}
```

The ==Data== field is the value each node contains (typically an numeral, but can be anything), and ==next== is a reference to the next **Node** inside the linked list.

A linked list can therefore be visualized as a collection of these nodes:

```csharp
Node(-35) -> Node(63) -> Node(-22) -> Node(91)
```

```csharp
var n1 = new Node(-35);
var n2 = new Node(63);
var n3 = new Node(-22);
var n4 = new Node(91);

n1.next = n2;
n2.next = n3;
n3.next = n4;
```

The **first** node in a linked list is called the head, and the **last** node is called the tail.

![[single_ll.jpg|500]]

Given just the head of the linked list, you can find the values for the rest of the nodes by continuously referencing the ==.next()== property. In other words, you can **traverse the linked list in _only_ one direction (left -> right)**.

```csharp
head.data == -35;
head.next.data = 63;
head.next.next.data == -22
head.next.next.next.data == 91;
```

Adding new nodes to a linked list can be tedious since you need to instantiate it and then also connect it. An easier way would be to create a **LinkedList** class to hold them. You can also define certain methods for convenience:

```csharp
public class LinkedList {

	private Node _head;

	public Node Find(int value){
		// finds a node with value
	}

	public void Add(int value){
		// adds new node and connects to tail
	}

	public void Delete(int value){
		// delete node(s) with specified value
	}
}
```


## Doubly linked list

A `doubly linked list` is a linked list where each node also has a reference to the previous node.

It can be defined like this:

```csharp
public class Node {
	int data;
	Node next;
	Node prev;

	public Node(int _data=0, Node _next = null, Node _prev = null){
		data = _data;
		next = _next;
		prev = _prev;
	}
}
```

```csharp
var n1 = new Node(-35);
var n2 = new Node(63);
var n3 = new Node(-22);
var n4 = new Node(91);

n1.next = n2;

n2.next = n3;
n2.prev = n1;

n3.next = n4;
n3.prev = n2;

n4.prev = n3;
```

