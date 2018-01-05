---
title: "Programmeringsuppgift 7"
date: 2016-06-02
tags: ["c++", "programmeringsuppgifter", "160602"]
draft: false
---

Copy assignment8.cc from given_files to your working directory. Add your own code according to instructions below and submit as Assignment #8.

The following function for sorting int values in range ``[begin, end)`` is given:
```
void sort(int* begin, int* end) {
    for ( ; begin +1 != end; ++begin) {
        int* min = begin;
        for (int* pos = begin + 1; pos != end; ++pos)
            if (*pos < *min) 
                min = pos;
            std::iter_swap(begin, min); 
    }
}
```
It could be used as follows for sorting the elements of an array:
```
int num[]{ 7, 5, 10, 432, 1, 2, 3, 4, 5 }; 
sort(begin(num), end(num));
```

Generalize sort in the following ways:

- any type fulfilling the requirements shall be possible to use
- the ordering operation, now \<, shall be replaced by an ordering policy
- make it work also for real iterators, not only for plain pointers (such as int*)

Do this by encapsulating sort in a template class named Sort. Sort shall have two template parameters, one for the type of values to sort, and the other for the ordering policy, see below. Leave the problem of making it work for iterators until later!

Define two sort policy classes, ``Ascending`` (normal sort order) and ``Descending``:

- With policy ``Ascending`` elements shall be sorted in ascending order, i.e. as the given
code do. This shall be the default policy for ``Sort``.
- With policy ``Descending`` elements shall be sorted in descending order.
After this, ``Sort`` should be possible to use on arrays in the following ways (Note: these examples gives an important hint about implementation):
```
Sort<int, Ascending>::sort(begin(num), end(num)); 
Sort<int, Descending>::sort(begin(num), end(num)); 
Sort<int>::sort(begin(num), end(num)); // default used
```
Now, modify ``Sort`` so iterators, e.g. container iterators, can be used:
```
vector<int> v(begin(num), end(num));
...
Sort<int, Descending>::sort(begin(v), end(v));
```
Uncomment the corresponding test code in ``main()`` for the final test.


## Given files
```
#include <iostream>
using namespace std;


void sort(int* begin, int* end)
{
    for ( ; begin +1 != end; ++begin)
    {
        int* min = begin;
        for (int* pos = begin + 1; pos != end; ++pos)
            if (*pos < *min)
                min = pos;
        std::iter_swap(begin, min);
    }
}

int main()
{
    int arr[] = {2,3,5,1,6,8};
    ::sort(begin(arr), end(arr));
    bool first {true};
    for ( auto i : arr )
    {
        if ( !first )
            cout << ", ";
        first = false;
        cout << i;
    }
    cout << endl;

    /*
    vector<int> values {2,3,6,8,3};
    Sort<int, Ascending>::sort(begin(values), end(values));

    list<string> words {"hi", "hello", "all", "students"};
    Sort<string, Descending>::sort(begin(words), end(words));
    */
}
```

## facit
```
#include <vector>
#include <list>
#include <iostream>
#include <algorithm>
using namespace std;

struct Ascending
{
    template <typename T>
    static bool less(T const & lhs, T const & rhs)
    {
        return lhs < rhs;
    }
};

struct Descending
{
    template <typename T>
    static bool less(T const & lhs, T const & rhs)
    {
        return lhs != rhs && rhs < lhs;
    }
};

template <typename T, class Order_Policy=Ascending>
struct Sort
{
    template <typename Iter>
    static void sort(Iter begin, Iter end)
    {
        for ( ; next(begin) != end; ++begin)
        {
            Iter min = begin;
            for (Iter pos = next(begin); pos != end; ++pos)
                if ( Order_Policy::less(*pos, *min) )
                    min = pos;
            std::iter_swap(begin, min);
        }
    }
};

// a general print function to see that the sorting works.
// This was not required.
template <typename Cont>
void print(Cont const & arr)
{
    bool first {true};
    for ( auto i : arr )
    {
        if ( !first )
            cout << ", ";
        first = false;
        cout << i;
    }
    cout << endl;
}
int main()
{
    int arr[] = {2,3,5,1,6,8};
    Sort<int>::sort(begin(arr), end(arr));
    print(arr);

    vector<int> values {2,3,6,8,3};
    Sort<int, Ascending>::sort(begin(values), end(values));
    print(values);

    list<string> words {"hi", "hello", "all", "students"};
    Sort<string, Descending>::sort(begin(words), end(words));
    print(words);
}
``