# algorithm

> 参考：[algorithm](http://www.cplusplus.com/reference/algorithm/)

## 1 Non-modifying sequence operations

### 1.1 all_of

**Test condition on all elements in range**

Returns true if pred returns true for all the elements in the range [first,last) or if the range is empty, and false otherwise.

```cpp
template <class InputIterator, class UnaryPredicate>
bool all_of (InputIterator first, InputIterator last, UnaryPredicate pred);
```

**Example**

```cpp
// all_of example
#include <iostream>     // std::cout
#include <algorithm>    // std::all_of
#include <array>        // std::array

int main () {
  std::array<int,8> foo = {3,5,7,11,13,17,19,23};

  if ( std::all_of(foo.begin(), foo.end(), [](int i){return i%2;}) )
    std::cout << "All the elements are odd numbers.\n";

  return 0;
}
```

### 1.2 any_of

```cpp
template <class InputIterator, class UnaryPredicate>
bool any_of (InputIterator first, InputIterator last, UnaryPredicate pred);
```

**Test if any element in range fulfills condition**

Returns true if pred returns true for any of the elements in the range [first,last), and false otherwise.

```cpp
// any_of example
#include <iostream>     // std::cout
#include <algorithm>    // std::any_of
#include <array>        // std::array

int main () {
  std::array<int,7> foo = {0,1,-1,3,-3,5,-5};

  if ( std::any_of(foo.begin(), foo.end(), [](int i){return i<0;}) )
    std::cout << "There are negative elements in the range.\n";

  return 0;
}
```

### 1.3 none_of

```cpp
template <class InputIterator, class UnaryPredicate>
bool none_of (InputIterator first, InputIterator last, UnaryPredicate pred);
```

**Test if no elements fulfill condition**

Returns true if pred returns false for all the elements in the range [first,last) or if the range is empty, and false otherwise.

**Example**

```cpp
// none_of example
#include <iostream>     // std::cout
#include <algorithm>    // std::none_of
#include <array>        // std::array

int main () {
  std::array<int,8> foo = {1,2,4,8,16,32,64,128};

  if ( std::none_of(foo.begin(), foo.end(), [](int i){return i<0;}) )
    std::cout << "There are no negative elements in the range.\n";

  return 0;
}
```

### 1.4 for_each

**Apply function to range**

Applies function fn to each of the elements in the range [first,last).

```cpp
template <class InputIterator, class Function>
Function for_each (InputIterator first, InputIterator last, Function fn);
```

**Example**

```cpp
// for_each example
#include <iostream>     // std::cout
#include <algorithm>    // std::for_each
#include <vector>       // std::vector

void myfunction (int i) {  // function:
  std::cout << ' ' << i;
}

struct myclass {           // function object type:
  void operator() (int i) {std::cout << ' ' << i;}
} myobject;

int main () {
  std::vector<int> myvector;
  myvector.push_back(10);
  myvector.push_back(20);
  myvector.push_back(30);

  std::cout << "myvector contains:";
  for_each (myvector.begin(), myvector.end(), myfunction);
  std::cout << '\n';

  // or:
  std::cout << "myvector contains:";
  for_each (myvector.begin(), myvector.end(), myobject);
  std::cout << '\n';

  return 0;
}
```

### 1.5 find

```cpp
template <class InputIterator, class T>
InputIterator find (InputIterator first, InputIterator last, const T& val);
```

**Find value in range**

Returns an iterator to the first element in the range [first,last) that compares equal to val. If no such element is found, the function returns last.

**Example**

```cpp
// find example
#include <iostream>     // std::cout
#include <algorithm>    // std::find
#include <vector>       // std::vector

int main () {
  // using std::find with array and pointer:
  int myints[] = { 10, 20, 30, 40 };
  int * p;

  p = std::find (myints, myints+4, 30);
  if (p != myints+4)
    std::cout << "Element found in myints: " << *p << '\n';
  else
    std::cout << "Element not found in myints\n";

  // using std::find with vector and iterator:
  std::vector<int> myvector (myints,myints+4);
  std::vector<int>::iterator it;

  it = find (myvector.begin(), myvector.end(), 30);
  if (it != myvector.end())
    std::cout << "Element found in myvector: " << *it << '\n';
  else
    std::cout << "Element not found in myvector\n";

  return 0;
}
```

### 1.6 find_if

```cpp
template <class InputIterator, class UnaryPredicate>
InputIterator find_if (InputIterator first, InputIterator last, UnaryPredicate pred);
```

**Find element in range**
Returns an iterator to the first element in the range [first,last) for which pred returns true. If no such element is found, the function returns last.

**Example**

```cpp
// find_if example
#include <iostream>     // std::cout
#include <algorithm>    // std::find_if
#include <vector>       // std::vector

bool IsOdd (int i) {
  return ((i%2)==1);
}

int main () {
  std::vector<int> myvector;

  myvector.push_back(10);
  myvector.push_back(25);
  myvector.push_back(40);
  myvector.push_back(55);

  std::vector<int>::iterator it = std::find_if (myvector.begin(), myvector.end(), IsOdd);
  std::cout << "The first odd value is " << *it << '\n';

  return 0;
}
```

### 1.7 find_if_not

```cpp
template <class InputIterator, class UnaryPredicate>
InputIterator find_if_not (InputIterator first, InputIterator last, UnaryPredicate pred);
```

**Find element in range (negative condition)**

Returns an iterator to the first element in the range [first,last) for which pred returns false. If no such element is found, the function returns last.

**Example**

```cpp
// find_if_not example
#include <iostream>     // std::cout
#include <algorithm>    // std::find_if_not
#include <array>        // std::array

int main () {
  std::array<int,5> foo = {1,2,3,4,5};

  std::array<int,5>::iterator it =
    std::find_if_not (foo.begin(), foo.end(), [](int i){return i%2;} );
  std::cout << "The first even value is " << *it << '\n';

  return 0;
}
```

### 1.8 find_end

查找最后一个匹配的子序列。

**Find last subsequence in range**
Searches the range [first1,last1) for the last occurrence of the sequence defined by [first2,last2), and returns an iterator to its first element, or last1 if no occurrences are found.

The elements in both ranges are compared sequentially using operator== (or pred, in version (2)): A subsequence of [first1,last1) is considered a match only when this is true for all the elements of [first2,last2).

**Example**

```cpp
// find_end example
#include <iostream>     // std::cout
#include <algorithm>    // std::find_end
#include <vector>       // std::vector

bool myfunction (int i, int j) {
  return (i==j);
}

int main () {
  int myints[] = {1,2,3,4,5,1,2,3,4,5};
  std::vector<int> haystack (myints,myints+10);

  int needle1[] = {1,2,3};

  // using default comparison:
  std::vector<int>::iterator it;
  it = std::find_end (haystack.begin(), haystack.end(), needle1, needle1+3);

  if (it!=haystack.end())
    std::cout << "needle1 last found at position " << (it-haystack.begin()) << '\n';

  int needle2[] = {4,5,1};

  // using predicate comparison:
  it = std::find_end (haystack.begin(), haystack.end(), needle2, needle2+3, myfunction);

  if (it!=haystack.end())
    std::cout << "needle2 last found at position " << (it-haystack.begin()) << '\n';

  return 0;
}
```

### 1.9 find_first_of

**Find element from set in range**

Returns an iterator to the first element in the range [first1,last1) that matches any of the elements in [first2,last2). If no such element is found, the function returns last1.

**Example**

```cpp
// find_first_of example
#include <iostream>     // std::cout
#include <algorithm>    // std::find_first_of
#include <vector>       // std::vector
#include <cctype>       // std::tolower

bool comp_case_insensitive (char c1, char c2) {
  return (std::tolower(c1)==std::tolower(c2));
}

int main () {
  int mychars[] = {'a','b','c','A','B','C'};
  std::vector<char> haystack (mychars,mychars+6);
  std::vector<char>::iterator it;

  int needle[] = {'A','B','C'};

  // using default comparison:
  it = find_first_of (haystack.begin(), haystack.end(), needle, needle+3);

  if (it!=haystack.end())
    std::cout << "The first match is: " << *it << '\n';

  // using predicate comparison:
  it = find_first_of (haystack.begin(), haystack.end(),
                      needle, needle+3, comp_case_insensitive);

  if (it!=haystack.end())
    std::cout << "The first match is: " << *it << '\n';

  return 0;
}
```

### 1.10 adjacent_find

**Find equal adjacent elements in range**

Searches the range [first,last) for the first occurrence of two consecutive elements that match, and returns an iterator to the first of these two elements, or last if no such pair is found.

```cpp
// adjacent_find example
#include <iostream>     // std::cout
#include <algorithm>    // std::adjacent_find
#include <vector>       // std::vector

bool myfunction (int i, int j) {
  return (i==j);
}

int main () {
  int myints[] = {5,20,5,30,30,20,10,10,20};
  std::vector<int> myvector (myints,myints+8);
  std::vector<int>::iterator it;

  // using default comparison:
  it = std::adjacent_find (myvector.begin(), myvector.end());

  if (it!=myvector.end())
    std::cout << "the first pair of repeated elements are: " << *it << '\n';

  //using predicate comparison:
  it = std::adjacent_find (++it, myvector.end(), myfunction);

  if (it!=myvector.end())
    std::cout << "the second pair of repeated elements are: " << *it << '\n';

  return 0;
}
```

### 1.11 count

### 1.12 count_if

### 1.13 mismatch

### 1.14 equal

### 1.15 search

### 1.16 search_n

## 2 Modifying sequence operations

## 3 Partitions

## 4 Sorting

## 5 Binary search

## 6 Merge 

## 7 Heap

## 8 Min/max

## 9 Other
