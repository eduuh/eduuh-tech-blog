---
title: C++ Fast Enviroment Setup
date: '2022-03-13'
tags: ['dsa']
draft: false
summary: A quick way to create C++ Application for Competitive Programming question. Give yourself time to think and remove the boilerplate out of the way.
images: []
layout: PostLayout
authors: ['default']
---

### Requirement

I need a way to create programs locally very fast with `no thought`. To solve and be able to run multiple tests on the questions.

### Solution

- Create a simple shell script that will create me three files in the folder **input.txt**, **ouput.txt** and **program.cpp**
- Make sure that the shell script in my path `/.local/bin`
- The C++ program should read in `STDIN` from the `input.txt`
- The C++ programm should read in `STDOUT` from the `output.txt`
- The C++ program should on the inputs in the **input.txt**. The first line ently with be the number of inputs in the input file.
- Building the C++ program with `debug symbols` and creating configured `.vimspector.json`.
- Use the `STDERR` to show the time taken to run the program. I know in programming doing theoritical analysis is the best way to compare the perfomance of the program i.e `using Big O notation`. I will have the time here since most of the online platform algorithms are judged by the time they take.

#### input.txt

This is the file used to cache the inputs to the program. Each line represent a **STDIN** equivalent to c++ **cin**.

- The first line is the number of the inputs in the input file.
- The second line is the start of the entry

An example content input.txt

```txt:input.txt
2
3
1 2 3
5
1 2 3 4 5
```

> Explanation

The input.txt content are taken from a program that expects an array of numbers. Then reverse the elements.

- The first line shows the number of **inputs**.i.e `2`
- The second line lets the number of elements in the array. i.e `3`
- The line after have the array numbers listed out. i.e `1 2 3`.

#### output.txt

This file will have the output of the program run.

```txt:output.txt
3 2 1
5 4 3 2 1
```

### program.cpp

The program file should have some include directives.

1. `#include <algorithm>` . Some algorithm in the c++ STL. i.e `sorting , Reversing, Swap`
2. `#include <bits/stdc++.h>`. #include of all major containes int C++ STL.
3. `using namespace std;`. This line helps remove verbosity when using C++ stl Containers.

Later when confortable with c++ stl i will add a section for `macros`. For now i will continue typing them I dont want to develop some bad habits when starting out doing c++.

```C
#define int long long int
#define F first;
#define S second;
#define pb push_back
```

The `main function`, here the stdin and stdout are redirected to our **input.txt** and **output.txt** respectively.

```c
int main() {
  freopen("input.txt", "r", stdin);
  freopen("output.txt", "w", stdout);

  return 0;
}

```

For the requirement to keep `track of time`. The way I go around this is to get the ticks when starting the program and the ticks when the program is stopping. The subtraction of this two values is then divided by **CLOCKS_PER_SEC** to convert the value to miliseconds. The output is printed out to `STDERR` stream using `cerr`.

The **#include** file for the `clock_t` is included in the `#include <bits/stdc++.h>` header file.

```c
int main() {
   ...


  clock_t z = clock();
  cerr << "Run Time :" << ((double)(clock() - z) / CLOCKS_PER_SEC);
  return 0;
}
```

In order to interate on all the inputs. We need a while loop and to be able to read the first `cin` value from the input file.

```c
int main() {
  ...
    int t = 1;
    cin >> t;
    while(t--) solve();
  ...
 return 0;
}
```

The solutions or **Algorithms I come up with** are always going to be in the `solve()` function. Example of a solve function.

```c
void solve() {
  int n;
  cin >> n;
  int a[n];
  for(int i =0; i<n; i++){
    cin >> a[i];
  }
  reverse(a,a+n);
  for (int i = 0;  i<n ; i++)
  {
     cout << a[i] << " ";
  }
  cout << '\n';
}
```

### The script

- Needed a way to tear down fast.
- Create a good starting point.
- Including `.vimspector.json` debugging configuration file.

```bash
#!/bin/bash
append() {
  echo "
  #include <bits/stdc++.h>
  using namespace std;

  void solve() {
  }

  int32_t main(int32_t argc, const char** argv) {
  freopen(\"input.txt\", \"r\", stdin);
  freopen(\"output.txt\", \"w\", stdout);
  clock_t z = clock();

  int t = 1;
  cin >> t;
  while(t--) solve();

    cerr << \"Run Time :\" << ((double)(clock() - z) / CLOCKS_PER_SEC);
    return 0;

  }

  " >> program.cpp
}

debugging() {
  echo "
{
  \"configurations\": {
    \"Launch\": {
      \"adapter\": \"vscode-cpptools\",
      \"configuration\": {
        \"name\": \"cpp: Launch current file\",
        \"request\": \"launch\",
        \"type\": \"cppdbg\",
        \"program\": \"/tmp/test\",
        \"MIMode\": \"gdb\",
        \"stopOnEntry\": true,
        \"stopAtEntry\":true,
        \"miDebuggerPath\": \"/usr/bin/gdb\",
        \"cwd\":\"\${cwd}\"
      }
    }
  }
}
  " >> .vimspector.json
}

clean() {
  [ -f input.txt ] && rm input.txt
  [ -f output.txt ] && rm output.txt
  [ -f program.cpp ] && rm program.cpp
  [ -f .vimspector.json ] && rm .vimspector.json
}

clean
touch input.txt output.txt program.cpp .vimspector.json
append
debugging
```

#### Running and Compilation

- I also need this to be fast.

I create an **nvim** solution. I know this is very tied to the editor. I created a `Run` command for compilation of cpp with debug symbols.

```lua
vim.cmd([[
   command! -nargs=? Run :!g++ -g % -o /tmp/test && /tmp/test <CR>
]])

```

#### To get Updated Version of the Script

The final script can be found [here]()
