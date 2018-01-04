---
title: "Programmeringsuppgift 7"
date: 2016-03-31
tags: ["c++", "programmeringsuppgifter", "160331"]
draft: false
---

Copy the file given_files/assignment7.cc to your working directory and make changes to your copy. Submit your answer as Assignment #7.

There is a function that works a bit like the standard algorithm ``std::find`` (but much simpler).
```
int * Find(int * start, int * end, int val) {
    for ( auto it = start; it != end; ++it ) {
        if ( *it == val ) {
            return it; }
        }
    }
    return end; // wasn't found
```
Your task is to do the following modifications to the given function so that the commented lines in the given code compile and give output according to the comments.

- Create a namespace called ``assignment`` and rename the given function to ``find`` and add it to that namespace to get rid of possible name clashes with ``std::find``.

- Make ``assignment::find`` into a template that works with real iterators instead of just pointer to int.

- Change the third argument of ``assignment::find`` to a unary predicate function (a function taking one argument and returning bool). ``find`` shall return the first element for which the predicate returns true (or the second argument if none is found).

With these alterations, we have gone from a very basic linear find that searches for a specific value of type int to a version of ``std::find_if``.


## Given files
```
int * Find(int * start, int * end, int val)
{
    for ( auto it = start; it != end; ++it )
    {
        if ( *it == val )
            return it;
    }
    return end; // wasn't found
}

int main()
{
    int arr [] = {1, 2, 5, 2, 7 ,8};
    auto pos = Find(begin(arr), end(arr), 2);
    if ( pos != end(arr) )
        cout << *pos << " found";
    else
        cout << "not found";
    
    // After modifications:
    /*
    std::list<int> l{1,6,2,7,0,9};
    auto val = assignment::find(begin(l), end(l),
                                [](int i) { return i % 2 == 0; });
    if ( val != end(l) )
        cout << "First even number: " << *val << endl;
    */
}
```

## Facit:

```
#include <iostream>
#include <list>
using namespace std;

namespace assignment
{
    template <typename Iter, typename Func>
    Iter find(Iter start, Iter end, Func fun)
    {
        for ( auto it = start; it != end; ++it )
        {
            if ( fun(*it) )
                return it;
        }
        return end; // wasn't found
    }
}

int main()
{
/*    int arr [] = {1, 2, 5, 2, 7 ,8};
    auto pos = Find(begin(arr), end(arr), 2);
    if ( pos != end(arr) )
        cout << *pos << " found";
    else
        cout << "not found";
*/
    // After modifications:

    std::list<int> l{1,6,2,7,0,9};
    auto val = assignment::find(begin(l), end(l),
                                [](int i) { return i % 2 == 0; });
    if ( val != end(l) )
        cout << "First even number: " << *val << endl;

}

```
