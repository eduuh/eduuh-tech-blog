---
title: Choosing the Right Collection .NET
date: '2022-02-02'
tags: ['CSharp', 'dsa']
draft: false
summary: Comparison of various collection, their internal data structures, performance and their usages.
images: []
layout: PostLayout
---

## C# Collection Framework

A "collection" in C# programming language is a group of **elements**, such as intergers or strings organized in a List , a Hast Table or some other data structures. C# collection framework is a set of high quality, high performance & reusalbe data-structures and algorithms.

The following advantages of using a C# collection framework:

1. The programmers do not have to implement basic data structures and algorithm repeatedly. Prevent reiventing the wheel. Keeping it DRY(Don't repeat yourself)
2. The C# Collection Framework code is well-tested, high quality, high performance code therefore increasing the quality of the programs.
3. Easy for review and understanding others programs as other developers also use the collection framework.
4. Collection Framework is well documented.

## Array

- Array represent a collection of multiple elements of same datatypes (Most common data structure used to store data.).
- Arrays are fixed size data structure, the size of this data structure is fixed at the time of creation.

Note: As we cannot change the size of an array, we generally declare a large size array to handle any future data expasion. This ends up in creating a large size array, where most of spaces is unused.

Arrays can store a fixed number of elements , whereas a collection store object dynamically so there is no size restrictions it grows and shrinks automatically.

### Array ADT Operations

Below is the API of the array: I will go into details of the Array ADT when creating my own array.

1. Adds an element at kth position

   Value can be stored in array at the Kth position in 0(1) constant time.

2. Reading the values stored at kth position.

   Accessing valuse stored at some location in the array in also 0(1) constant time. We just need to read the value stored at arr[K].

C# standard arrays are of fixed length. Sometime we do not know how much memory we need so we create a bigger size array. There by wasting space to avoid this situation C# Collection framework had implemented List to solve this problem.

## List\<T\>

List\<T\> in C# collection is a data-structures which implement IList\<T\> interface Which means that it can have duplicate elements in it. List is an implementation as dynamic array that can grow or shrink as needed. Internally array is used when it is full a bigger array is allocated and the old array element are copied over.

Below is the list of few most frequently used properties and methods of List\<T\> class:

| Properties & Methods | Description                                       |
| -------------------- | ------------------------------------------------- |
| Count                | Get the number contained in the list              |
| item[index]          | Get or sets the element at the specified index    |
| Add                  | Adds an object to the end of the List             |
| Clear                | Removes all Elements from the List                |
| Remove(T)            | Remove the first occurence object T from the list |
| RemoveAt(index)      | Removes the element at the specified index        |
| Reverse()            | Reverse the order of the element in the List      |

## SortedList\<T\>

- A SortedList\<T\> is a data structure that maps keys to values.
- A SortedList\<T\> is an implementation of IDictionary\<T\> and is implemented using a sorted array so the key-values pairs are stored in sorted order.
- SortedList\<T\> does not allow duplicate keys but values can be duplicate. Since key/value pairs are stored in a sorted array, so key searching is performed using **binary search**.

Below is the list of few most frequently used properties and methods of SortedList\<T\> class:

| Properties & Methods | Description                                                  |
| -------------------- | ------------------------------------------------------------ |
| Count                | Get the number of key/value pair contained in the Dictionary |
| item[key]            | Get or sets the value associated with the key                |
| Add(key, value)      | Adds a key/value pair to the Dictionary                      |
| Clear()              | Removes all key/value pairs from the Dictionary              |
| ContainsKey(key)     | Check whether a particular key is present in the Dictionary  |
| Remove(key)          | Remove the key/value pair from the Dictionary                |

## Linked List

Linked list are dynamic data structures and memory is allocated at run time. The concept of linked list is not to store data contiguously. Use links that points to the next elements.
performance wise linked list are slower than arrays because there is no direct access to linked list elements.

- The linked lisit is useful data structure when we do not know the number of elements to be stored ahead of time.
- There are many flavours of linked list: **Linear**, **Circular** , **doubly** and **doubly circular**.

### Linked List ADT operations

1. **Insert(k)**: Adds k to the start of the list:

   Insert an element at the start of the list. Just create a new element and move references. So that this new element becomes the new element of the list. This operation takes 0(1) constant time.

2. **Delete()**: Delete element at the start of the list.

   Delete an element at the start of the list. We just need to move one reference. This operation will also take 0(1) constant time.

3. **PrintList()**: Displays all the elements of the list.

   Start with the first element and then follow the references. This operation will take 0(N) time.

4. **Find(k)**: Find the position of element with value K.

   Start with the first element and follow the reference until we get the value we are looking for or reach the end of the list. This operation will take 0(N) time.

5. **FindKth(K)**: Find element at position k.

   Start from the first element and follow the links until you reach the kth element. This operation will take 0(N) time.

6. **IsEmpty()**: Check if the number of elements in the list are zero.

   Just check the head reference fo the list it should be Null. Which means list is empty. This operation will take 0(1) time.

LinkedList\<T\> in C# is a data structure which implement linked list data structure. It is a **doubly linked list** you can add and remove from both ends of the list.

Below is the list of few most frequently used properites and methods of LinkedList\<T\> class:

| Properties & Methods | Description                                                            |
| -------------------- | ---------------------------------------------------------------------- |
| AddFirst(T)          | Adds a new node with specified value at the start of the LinKed List   |
| AddLast(T)           | Add a new node with specified value at the end of the Linked List      |
| Clear()              | Removes all node from the Linked List                                  |
| Find(T)              | Finds the first node that contains the specified value                 |
| Remove(T)            | Remove the first occurence of the specified value from the Linked List |
| RemoveFirst()        | Removes the node at the start of the Linked List                       |
| RemoveLast()         | Removes the node at the end of the Linked List.                        |

## Stack

Stack is a special kind of data-structures that follows the Last-in-First-Out strategy. This means that the element that is added to stack last will be the first to be removed.

Application of stacks includes:

1. **Recursion**: Recursive calls are performed using system stack.
2. Postfix evaluation of expression.
3. Backtracking.
4. Depth-First search of trees and graphs.
5. Conversion of a decimal Number into a binary number.

### Stack ADT

- **Push(k)** : Adds a new item to the top of the stack.
- **Pop()**: Removes an element from the top of the stack and return its value.
- **Top()**: Returns the value of the element at the top of the stack.
- **Size()**: Returns the number of elements in the stack.
- **IsEmpty()**: Determines whether the stack is empty . It returns 1 if the stack is empty or return 0.

Note: All the above stack operations are implemented in **0(1) Time Complexity**.

Stack is implemented by Stack\<T\> in C# collection with implement ICollection interface. Elements are added to the stack by calling Push() function and element are removed from the stack by calling Pop() fuction.

Below is the list of few most frequently used properties and methods of Stack\<T\> class.

| Properties & Methods | Description                                                     |
| -------------------- | --------------------------------------------------------------- |
| Count                | The number of elements contained in the stack                   |
| Clear()              | Removes all elements from the stack                             |
| Peek()               | Returns the element at the top of the Stack without removing it |
| Pop()                | Removes and returns the element at the top of the stack         |
| Push(T)              | Inserts an element at the top of the Stack                      |

## Queue

A queue is First-In-Firt-Out kind of data-structures. The element that is added to the queue first will be the first to be removed from the queue.

Queue has the following application users.

1. Access to shared resources (printer).
2. Message Queue.
3. Multiprogramming.

### Queue ADT Operation:

- **Add()**: Add a new element to the back of the queue.
- **Remove()** Remove an element from the front of the queue and return its value.
- **Front()** Return the value of the element at the front of the queue.
- **Size()** Returns the number of elements inside the queue.
- **IsEmpty()** return 1 if the queue is empty otherwise return 0.

Note: All the above queue operations are implemented in 0(1) Time complexity.

Queue\<T\> is the class implementaiton of Queue in C# collection.

Below is the list of few most frequently used properties and methods of Queue\<T\> class:

| Properties & Methods | Description                                               |
| -------------------- | --------------------------------------------------------- |
| Count                | The number of element contained in the queue              |
| Clear()              | Removes all element from the queue                        |
| Dequeue()            | Remove and returns the element at the front of the queue. |
| Enqueue(T)           | Add Element T to the back of the queue.                   |
| Peek()               | Return the element at the fromt of the queue              |

Note: The elements that are added to the queue first are the one that comes out of the queue first.

## Trees

Trees is a hierarchical data structure. The top element of tree is called the root of the tree. Except the root element, every element in the tree has a parent element, and zero or more child elements. The tree is the most useful data structure when you have hierachical information to store.

Types of trees includes:

1. binary-tree.
2. Red-black tree
3. AVL trees.

### Binary Trees

A binary tree is type of tree in which each node at most two children which are referred as left child and right child.

### Binary Search Trees (BST)

A binary search tree(BST) is a binary tree on which nodes are ordered in the following ways:

1. The key in the left subtree is lest than the key in its parent node.
2. The key inthe right subtree is greater or equal the key in its parent node.

#### Binary Search Tree ADT Operation

- **Insert(k)**: Insert an element k into the tree.
- **Delete(k)**: Delete an element k from the tree.
- **Search(k)**: Search a particular value k into the tree if it is present or not.
- **FindMax(k)**: Find the maximum value stored in the tree.
- **FindMin()**: Find the minimum valaue stored in the tree.

The average Time Complexity of all the above operations on binary search tree is **0(log n)**, the case when the tree is balanced.

The worst case Time complexity will be **O(n)** when the tree is skewed. A binary tree is skewed when tree is not balanced.

There are two types of skewed tree.

1. Right skewed binary tree:

   A binary tree in which each node is having either only a right child or no child at all.

2. Left skewed binary tree:

   A binary tree in which each node is having either only a left child or no child at all.

### Balanced Binary Search Tree

There are few binary search tree, which always keep themselves balanced.

Most important among them are:

- Red-black Tree (RB-Tree)
- AVL tree.

#### SortedSet\<T\>

SortedList\<T\> is a class which implement ISet\<\> interface which means that it store only unique elements.SortedList\<T\> is implemented using **a red-black balanced binary tree** in C# collections. Since SortedList\<T\> is impemented using a binary search tree, its elements are stored in sequential order.

Below is the list of few most frequently used properties and methods of SortedList\<T\> class.

| Properties & Methods | Description                                          |
| -------------------- | ---------------------------------------------------- |
| Count                | Get the number of element in set                     |
| Add(T)               | Add an element to the set                            |
| Clear()              | Remove all element from the set                      |
| Contain(T)           | Check whether specific element is present in the set |
| Remove(T)            | Remove a specific element from the set               |

#### SortedDictionary\<T\>

An IDictionary\<T\> is an interface that maps keys to values. A SortedDictionary\<\> is an implementation of IDictionary\<\> and is implemented using **red-black balanced binary search tree** so the key value pair are stored in sorted order.

Below is the list of few most frequently used properties and methods of SortedDictionary\<T\> class:

| Properties & methods | Description                                                  |
| -------------------- | ------------------------------------------------------------ |
| Count                | Get the number of key/value pair contained in the Dictionary |
| Item[key]            | Get or Set the value associated with the key                 |
| Add(key, value)      | Add a key/value pair to the Dictionary                       |
| Clear()              | Remove all the key/value pair from the Dictionary            |
| ContainsKey(key)     | Check whether a particular Key is present in the Dictionary  |
| Remove(key)          | Remove the key/value pair from the Dictionary                |

## Hash Table

A hash-Table is a data-structures that maps keys to values. Each position of the Hash-Table is called a **slot**. The hash-Table uses a hash function to calculate an index of an array of slots. We use Hash Table when the number of keys actually stored is small relatively to the number of possible keys.

The process of storing objects using a hash function is as follows:

1. Create an array of Size m to store objects. This array is called Hash-Table.
2. Find a hash code of an object by passing in through the hash function.
3. Take module of hash code by the size of Hash-Table to get the index of the table where objects will be stored.
4. Finally store these objects in the designated index.

The process of searching object in hash table using a hash function is as follows.

1. Find a hash code of the object we are searching for by passing it through the hash function.
2. Take module of hash code by the size of Hash-Table to get the index of the table where objects are stored.
3. Finally, retrive the object from the designated index.

### Hash-Table ADT

ADT of hash-Table contains the following functions.

- **Insert(x)**: Add object x to the data set.
- **Delete(x)**: Delete object x from the data set.
- **Search(x)**: Search object x in data set.

The hash-Table is a useful data structure for implementing dictionary. The average time to search for an element in a Hash-Table is **O(1)**.

HashSet\<T\> is a class which implements ISet\<T\> interface which means that it store only unique elements. HashSet\<T\> is implemented using a hash table. Since HashSet\<\> is implemented using a hash table its elements are not stored in sequential order.

Below is the list of few most frequently used properties and Methods:

| Properties & Methods | Description                                          |
| -------------------- | ---------------------------------------------------- |
| Count                | Get the number of element in set                     |
| Add(T)               | Add an Element to the set                            |
| Clear()              | Remove all elements from the set                     |
| Constains(T)         | Check whether specific element is present in the set |
| Remove(T)            | Remove a specific element from the set.              |

### Dictionary

A Dictionary\<T\> is a data structure that maps keys to values. A Dictionary\<T\> is an implementation of IDictionary\<T\> and is implemented using a hash table so the key values pairs are not stored in sorted order. Dictionary\<T\> does not allow duplicate keys but values can be duplicate.

Below is the list of few most frequently used properties and method of Dictionary\<T\> class:

| Properties & Method | Description                                                  |
| ------------------- | ------------------------------------------------------------ |
| Count               | Get the number of key/value pair contained in the Dictionary |
| item[key]           | Get or set the value associated with the Key                 |
| Add(key, value)     | Add a key/value pair to the Dictionary                       |
| Clear()             | Remove all tke key/value pairs from the Dictionary           |
| ContainsKey()       | Check whether a particular key is present in the Dictionary  |
| Remove(key)         | Remove the key/Value pair form the Dictionary                |
