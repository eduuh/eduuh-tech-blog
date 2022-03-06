---
title: C++ Standard Template Library (STL) and Algorithms
date: '2022-03-03'
tags: ['dsa', 'C++']
draft: false
summary: Learning the Cpp standard Library. Data Structures and Algorithms
images: []
layout: PostLayout
---

# Generic Programming

A programing paradigm where data types are not specified in the implementation of code, but rather in it use. Sometime refered to **Compile-Time polymorphism**. Specifying data types at variable definition of objects instantiation is, in essence, polymorphism resolved at compile time.

This is potentially more efficient than runtime polymorphism.

**C++** enables generic programming by use **Templates constructs**.

## Templates

Templates allow you to write generics. Templates are used to define fuctions or classes.

```c
template <typename T>
int size_in_bits(const T &a) {
  return sizeof(a)*8;
}

template <typename T>
T maxof( const T & a, const T & b) {
  return (a > b ? a: b);
}


void size_inbits() {
  cout << endl<<endl <<endl;
  cout << "Interger :" << size_in_bits(24) << endl;
  cout << "Character :" << size_in_bits('f') <<endl;
  cout << "Floats: " << size_in_bits(32.2f) <<endl;
  cout << "Decimals: "<<size_in_bits(32.1) <<endl;
}

int main() {
   int a = 7;
   int b = 8;

    cout << "max is " << maxof<int>(a, b) << endl;
    size_inbits();
  }
```

The compiler generate the desired code when using the template.
In case of **int** usage as shown above.

```c
int maxof( const int & a, const int & b) {
  return (a > b ? a: b);
}
```

When creating templates the **typename** is interchangerble with the **class**.

```c
template <class T>
T maxof( const T & a, const T & b) {
    return (a > b ? a: b);
}
```

C++ 14 provides a new implementation for variables.

#### Type Inference

Type inference is **auto** in keyword.

```c
auto x = "this is a c-string";
```

We can use auto for template types , determine the resutant data type.

```C
template <typename LT, typename rT>

auto lf(const LT & lsh , const rT & rht) {
    return lsh + rht;
}
```

Using auto in Loops. Derives the types from the return types. This feature is used for convinience.

1. **Iterator loop**

```C
const string sclass = string("this is a string class string");

for(auto t: sclass) {
   cout << t << " ";
}

```

2. **Range based loop**

```c
const string sclass = string("this is a string class string");

for(auto t: sclass) {
   cout << t << " ";
}
```

#### Type Deduction

The compiler can determine the type of the generics from the passed types. This is called **type deduction**.

```C
cout << "max is " << maxof(a, b) << endl;
```

Templates are used mostly in stl containers.

### Advantages of stl

The stl enables us to create code.

- Quickly - datastructures are already implementations.
- Efficient code.
- Coherent code. - Most of c++ developer will write code usind stl
- Generic code. - Library are written as template. Lets the developers focus on the implementation other than the code setup.

## Components of the C++ STL

> ### Containers

- Array-like data structures that store collections of objects.

Containers are objects that handle a collection of other objects (elements), implementing a well-defined data structures.

The three types of containers.

> ### Sequence
>
> Storage of data elements. Examples: **vectors, list , deques, stacks, queues**
>
> ### Associative

Storage of associative pairs implemented as trees.

Examples: **Sets, maps, multisets , multimaps**

> ### Unordered Associative

Storage of associative pairs implemented as hash tables.

Examples: **Sets , maps , multisets , multimaps**

### Examples

> ### Vector

- **Dynamica size array**
- Supports random access for insertion and deletion of elements.
- Elements are stored contigously in memory.
- Dynamic resizes as needed.
- There is also array.

> ### List

- **Boubly linked list**
- Does not support random access of elements.
- Elements are not stored contigously in memory
- Different performanence than a vector.
- There is also forward_list.

> ### Deque

- **Double-ended queue**
- Support push and po operations on both ends.
- Elements are not all stored contigously in memory.
- Also serves as a **stack or queue** There are definition of these two in terms of **container adapters**.

> ### Containers Adapter

1. Stack. Push and Pop.
2. queue. Dequeue and Enqueue
3. Priority queue:

> ### Set

A set is a **Mathematica set**. Collection of unique elements. Types of sets.

> - Based on implementations

1. **Order sets** implemented as binary tree.
2. **Unorder sets** Unorodered sets as hash tables.

> Accepts duplicates or Does not accept duplicates.

1. **multisets** for duplicates.

> ### Maps

Sets of pairs. Pairs are two tuples. The two elements are called **first** and **second**.

- Maps implementations is similar to sets.
- For duplicates , Use multimaps.

## C++ Iterators

Pointer like object that allow travesal of containers in some order for either reading or writing. Iterators are defined as templates. Are **pointers and iterators** the same thing? **No**, pointers are one particular type of iterators.

Types of iteratos in the c++ STL:

1. _Input interator_

Read one way, one pass. Only able to read. Only moves forward. Only one pass possible. Least requirements. suibable for input streams

2. _Output interators_

Writing one way, one pass and only moves foward. Suitable for output stream such as screan text output.

3. _Forward iterators_

Read/write one way. Able to read and write.(mutable). Support multiple passess. Suitable for traversing singly linked lists.

4. _Bidirectional iterators_

Read/Write two way, multipass. Able to read and write (Mutable). Moves forward and backword. Support multiple passes. Suitable for traversing doubly linked lists.

5. _Random access interators_

Most capable interator. Read/Write anywhere (mutable). Able to access any element by index. Support multiple passes. Suitable for **Vectors and Arrays**.

#### Summary of interatos and what supports what.

| Iterator      | Container                                                                    |
| ------------- | ---------------------------------------------------------------------------- |
| Foward        | **forward_list** , **Unorodered\_[multiset]set**, **Unorodered\_[multi]map** |
| Bidirectional | **list** , [**multi**]**set**, [**multi**]**map**                            |
| Random Access | **Vector** , **deque**, **array**                                            |

#### Four Important Iterator Functions

They are implemented for all containers expect the _foward_list_.

- **begin()** Returns an iterator to the beginning of the container.
- **end()** Returns an iterator to the end of the container.
- **rbegin()** Returns a reverse iterator to the reverse beginning fo the container.
- **rend()** Returns a revese iterator to the reverse end of the container.

#### Iterator Invalidation

When there are changes in the contents of containers that is currently using iterators, these iterators may become invalid. The interator will no logger be guranteed to point to the same value.
