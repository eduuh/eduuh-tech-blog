---
title: C++ STL Containers Iterators
date: '2022-03-07'
tags: ['dsa']
draft: false
summary: Introduction to Iterators
images: []
layout: PostLayout
---

Iterators are used to point at the memory addresses of **STL Containers**. They reduce the complexity and execution time of programs.

## Operations of interators

- **begin()** Returns an iterator to the **beginning position** of the container.
- **end()** Returns an iterator to the **after end** of the container.

```c
#include <bits/stdc++.h>
using namespace std;
int main() {
    vector<int> v1 = {1, 2,3 , 4, 5, 6 , 7 , 8 , 9 , 10};
     //more preffered wayt to define them
     auto ibegin = v1.begin();
     auto iend = v1.end();
     for(auto it = ibegin; it < iend; ++it) {
        cout << *it << " ";
     }
}
```

###### Output:

```
1 2 3 4 5 6 7 8 9 10
```

- **advance():** This fuction is used to **increment the interator position** till the specified number mentioned in its arguments.

```c
#include <bits/stdc++.h>
using namespace std;
int main() {
    vector<int> v1 = {1, 2,3 , 4, 5, 6 , 7 , 8 , 9 , 10};
     auto ibegin = v1.begin();
     advance(ibegin, 3);
     cout << "The position of iterator after advancing is :" << "\n";
     cout << *ibegin << "\n";
}
```

###### Output:

```
The position of iterator after advancing is : 7
```

- **next()** Returns **the new iterator** that the interator would point after advancing the position mentioned in its arguments.
- **prev()** Returns **thhe new interator** that the iterator would point **after decrementing the position** mentioned in its arguments.

```c
// C++ code to demonstrate the working of
// next() and prev()
#include<iostream>
#include<iterator> // for iterators
#include<vector> // for vectors
using namespace std;
int main()
{
    vector<int> ar = { 1, 2, 3, 4, 5 };

    // Declaring iterators to a vector
    vector<int>::iterator ptr = ar.begin();
    vector<int>::iterator ftr = ar.end();


    // Using next() to return new iterator
    // points to 4
    auto it = next(ptr, 3);

    // Using prev() to return new iterator
    // points to 3
    auto it1 = prev(ftr, 3);

    // Displaying iterator position
    cout << "The position of new iterator using next() is : ";
    cout << *it << " ";
    cout << endl;

    // Displaying iterator position
    cout << "The position of new iterator using prev()  is : ";
    cout << *it1 << " ";
    cout << endl;

    return 0;
}
```

Output

```
The position of new iterator using next() is : 4
The position of new iterator using prev()  is : 3
```

- **inserter()** The function is used to insert an element at any position in the Containers. It accepts **2 arguments, the container and interator to the position where the element have to be inserted.**

```c
// C++ code to demonstrate the working of
// inserter()
#include<iostream>
#include<iterator> // for iterators
#include<vector> // for vectors
using namespace std;
int main()
{
    vector<int> ar = { 1, 2, 3, 4, 5 };
    vector<int> ar1 = {10, 20, 30};

    // Declaring iterator to a vector
    vector<int>::iterator ptr = ar.begin();

    // Using advance to set position
    advance(ptr, 3);

    // copying 1 vector elements in other using inserter()
    // inserts ar1 after 3rd position in ar
    copy(ar1.begin(), ar1.end(), inserter(ar,ptr));

    // Displaying new vector elements
    cout << "The new vector after inserting elements is : ";
    for (int &x : ar)
        cout << x << " ";

    return 0;
}
```

output:

```

```

- **rbegin()** Returns a reverse iterator to the reverse beginning fo the container.
- **rend()** Returns a revese iterator to the reverse end of the container.

## _Input interator_

Read one way, one pass. Only able to read. Only moves forward. Only one pass possible. Least requirements. suibable for input streams

```c
#include <bits/stdc++.h>
#include <iterator>
#include <iostream>
#include <locale>
using namespace std;

void message(const char * m) {
   cout << m << "\n";
}

int main(int argc, const char** argv) {
    double d1 = 0, d2 = 0;

    cout << "Two numeric values: " << "\n" << flush;
    cin.clear();

    istream_iterator<double> eos;      //default constructor for end of stream
    istream_iterator<double> iit(cin); //stdin iterator

    if(iit == eos) {
        message("no values");
        return 0;
    } else {
         d1 = *iit++;
    }

    if(iit == eos) {
       message("no second value");
       return 0;
    }else {
        d2 = *iit;
    }

     cin.clear();
     cout << d1 << " * " << d2  << "=" << d1 * d2 << "\n";

    return 0;
}
```

## _Output interators_

Writing one way, one pass and only moves foward. Suitable for output stream such as screan text output.

```c
#include <bits/stdc++.h>
#include <iterator>
using namespace std;

int main(int argc, const char** argv) {

    ostream_iterator<int> output(cout, " ");

    for(int i : { 3, 187, 45}) {
        *output++ = i;
    }
    cout << "" << "\n";
    return 0;
}
```

## _Forward iterators_

Read/write one way. Able to read and write.(mutable). Support multiple passess. Suitable for traversing singly linked lists.

```c
#include <bits/stdc++.h>
#include <iostream>
using namespace std;

int main(int argc, const char** argv) {
    //singly linked list
    forward_list<int> fl1 = { 1, 2, 3 , 4 , 5};

    forward_list<int>::iterator it;

    for(it = fl1.begin(); it != fl1.end(); ++it){
       cout << *it << "\n";
    }

    cout << "Range Based for Loop" << "\n";
    //range based forloop used a forward interator
    //read and write
    for(int i : fl1) {
        cout << i << "\n";
    }

    return 0;
}

```

## _Bidirectional iterators_

Read/Write two way, multipass. Able to read and write (Mutable). Moves forward and backword. Support multiple passes. Suitable for traversing doubly linked lists.

```c
#include <bits/stdc++.h>
#include <iostream>
using namespace std;

int main(int argc, const char** argv) {
    set<int> set1 = { 1, 2, 3 , 4 , 5 , 6 , 7 , 8 , 9 , 10 };
    set<int>::iterator it;

    cout << "/* forward direction */" << "\n";
    //iterate forward
    for (it = set1.begin(); it != set1.end(); ++it)
    {
       cout << *it << " ";
    }
    cout << "" << "\n";
    cout << "/* Backword direction */" << "\n";

    for (it = set1.end(); it != set1.begin();)
    {
       cout << *--it << " ";
    }

    cout << "" << "\n";
    cout << "Range based forloop" << "\n";
    for(int i : set1){
      cout << i << "\n";
    }

    return 0;
}

```

## _Random access interators_

Most capable interator. Read/Write anywhere (mutable). Able to access any element by index. Support multiple passes. Suitable for **Vectors and Arrays**.

```c
#include <bits/stdc++.h>
#include <iostream>
#include <iterator>
using namespace std;

void message(const char *m) { cout << m << "\n"; }
void message(const char *m , int i) { cout << m << ": " << i << "\n"; }


int main(int argc, const char** argv) {
    vector<int> v1 = { 1, 2, 3 , 4 , 5 , 6 , 7 , 8 , 9 , 10 };
    vector<int>::iterator it;

    cout << "Iterate Forward" << "\n";
    //interate forward
    for(it = v1.begin(); it != v1.end(); ++it) {
      cout << *it << "\n";
    }

    cout << "Iterate backwords" << "\n";

    for(it = v1.end(); it != v1.begin();) {
      cout << *--it << "\n";
    }

    cout << "Range Based interator" << "\n";
    for (int i : v1) {
        cout << i << "\n";
    }

    it = v1.begin() + 5;
    message("element begin + 5", *it);
    message("element [5]", v1[5]);
    it= v1.end() - 3;
    message("element end - 3 " , *it);
    return 0;
}
```
