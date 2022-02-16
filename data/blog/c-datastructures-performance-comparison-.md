---
title: C# DataStructures Performance Comparison
date: '2022-02-02'
tags: ['Csharp', 'dsa']
draft: false
summary: compare various collections, their internal data strucutrure, performance and their usage.
images: []
layout: PostLayout
---

## Overview of Data Structures

A way to look at data strcuturse is to focus on their strengths and weaknesses.

Characteristics of Data Structures.

| Data Structure | Advantages                                                                                                        | Disadvantages                                                                        |
| -------------- | ----------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| Array          | Quick Insertion <br/> Very fast access if index is known.                                                         | Slow Search <br/> Slow deletion <br/> fixed size                                     |
| Ordered Array  | Quicker Search than unsorted array (binary search)                                                                | Slow insertion and deletion, fixed size<br/>                                         |
| Stack          | Provides last-in , first out<br/>                                                                                 | Slow access to other elements                                                        |
| Queue          | Provide first-in, first out access                                                                                | Slow access to other elements                                                        |
| LinkedList     | Quick insertion <br/> Quick Deletion                                                                              | Slow Search                                                                          |
| Binary Tree    | Quick Search , insertions , deletions (if the tree remains balanced)                                              | Deletion Algorithm is complex                                                        |
| Red-Black tree | Quick Search , insertions , deletions (Tree always is balanced)                                                   | Complex                                                                              |
| 2-3-4 tree     | Quick Search, insertion, deletions. <br/> Tree is always balanced <br/> Similar trees good for disk storage <br/> | Complex                                                                              |
| Hash table     | Very fast access if key known. <br/> Fast Insertion                                                               | Slow deletion <br/> Slow access slow if key not known <br/> Inefficient memory usage |
| Heap           | Fast insertion, deletion, access to largest item.                                                                 | Slow access to other items                                                           |
| Graph          | Models real-world situations                                                                                      | Some algorithm are slow and complex                                                  |

## Internal DataStructure Used

Table comparison of underlying DataStructures and the ordering of elements.

| GenericCollection     | interface        | Internal DataStructures     | Ordering           |
| --------------------- | ---------------- | --------------------------- | ------------------ |
| List\<T\>             | IList\<T\>       | Array                       | Usage Dependent    |
| LinkedList\<T\>       | -                | Doubly Linked List          | Usage Dependent    |
| Stack\<T\>            | ICollection\<T\> | Array                       | Last In First Out  |
| Queue\<T\>            | ICollection\<T\> | Array                       | First In First Out |
| SortedList\<T\>       | -                | Sorted Array(binary search) | Sorted             |
| Dictionary\<T\>       | -                | Hash Table                  | Unordered          |
| SortedDictionary\<T\> | -                | Binary Search Tree          | Sorted             |
| HashSet\<T\>          | -                | Hash Table                  | Unordered          |
| SortedSet\<T\>        | -                | Binary Search Tree          | Sorted             |

## Time Complexities of Operations

| Generic Collection | Lookups                       | Insertions                                    | Deletion                                   |
| ------------------ | ----------------------------- | --------------------------------------------- | ------------------------------------------ |
| List\<T\>          | Index: O(1) <br/> Value: O(n) | O(n)                                          | O(n)                                       |
| LinkedList\<T\>    | Value: O(n)                   | Value: O(n)                                   | Value: O(n)                                |
| Queue\<T\>         | Front: O(1)                   | Enqueue: O(1)                                 | Dequeue: O(1)                              |
| Stack\<T\>         | Top: O(1)                     | Push: O(1)                                    | Pop: O(1)                                  |
| SortedList\<T\>    | O(log n)                      | Sorted input: O(1) <br/> Unsorted input: O(n) | Last Element: O(1) <br/> Index based: O(n) |
| Dictionary         | O(1)                          | O(1)                                          | O(1)                                       |
| SortedDictionary   | O(log n)                      | O(log n)                                      | O(log n)                                   |
| HashSet\<T\>       | O(1)                          | O(1)                                          | O(1                                        |
| SortedSet\<T\>     | O(log n)                      | O(log n)                                      | O(log n)                                   |

### Summary

#### List

- It is a wrapper around array.
- It is used when direct index based access is required and data is small.

#### LinkedList

- Doubly Linked List is the internal data structure.
- Used when both from and end insertion and deletion is required.

#### Stack

- Last In First Out DataStructure.
- Highly efficient with contant time access.

#### Queue

- First-In-First-Out data structure.
- Highly efficient with constant time access.

#### SortedList

- It is a sorted Array. Whenever data is added, it keeps the data in sorted order.
- Since data is kept in sorted order, insertion will be constly for random value with O(n).
  - This is due to alot of _shifting of elements is required_.
- If the insertions is of a sorted input or nearly sorted then insertion is efficient and need very less shifting of data.

- **Lookups** is done using the binary search algorithm which is efficient with O(log n) time complexities. Since data is stored in the array, the space requirement is minimum.
- SortedList is preferred when data is already sorted or when the number of insertion and deletion is rare compared with the number of lookups.

#### Dictionary

- Data is stored in using a hash table so insertion, deletion and indexing all are O(1) time complexity, considering there is no resizing required.

- Dictionary\<T\> is the fastest in term of insertion , deletion and lookups because it implements a hash table under the hood.

- The only downside of using Dictionary is that it store data unordered and it is difficult to traverse through all the data in a Dictionary.

- Since hash table is used to store data, the space requirement is more than sorted list.

#### SortedDictionary Index: O(1) / Index: O(1) /

- Data is stored using a height balanced search tree (Red-Black Tree) so insertion, deletion and indexing all are O(log n) time complexities.

- All the discussion of Dictionary\<T\> is applicable of HashSet\<T\> too, execpt that HashSet\<T\> is a set and is a colleciton of unique elements.

### Sorted Set

Data is stored using a height balanced binary search tree so insertion, deletion and indexing all are O(log n) time complexities.

All the discussion of SortedDictionary\<\> is applicable for SortedSet\<\> too, execpt that SortedSet\<\> is a set and is a collection of unique elements.
