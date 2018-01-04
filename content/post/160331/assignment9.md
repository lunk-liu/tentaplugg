---
title: "Programmeringsuppgift 9"
date: 2016-03-31
tags: ["c++", "programmeringsuppgifter", "160331"]
draft: false
---

Copy the file given_files/assignment9.cc to your working directory and make changes to that file. Submit your answer as Assignment #9.

This task is divided into two parts, the first giving three of the total five points and the second is worth the two remaining points. You are not supposed to give separate solutions for the two parts, but may stop after implementing the first part.

Your task in this assignment is to create a stream output manipulator to format a container for printing to some ostream. When done, the code in listing 2 shall compile and give output according to Listing 3.

*Listing 2: Code example for assignment 9*
```
int main() {
    vector<int> vec {2, 5, 1, 7, 10};
    cout << format_container () << vec << endl ; cout << format_container(’{’) << vec << endl;
}
```
    *Listing 3: output from listing 2*
    [2,5,1,7,10] 
    {2, 5, 1, 7, 10}

(a)

1. Create a simple struct called ``format_container` with three data members; a  pointer to an ``ostream``, and starting and ending delimiters. Add a constructor taking a char to set the starting delimiter (it’s enough to support brackets and braces) with brackets being the default.
2. Create an output operator that takes an ostream and a ``format_container`` that sets the ``ostream`` pointer for that ``format_container`` and returns it.
3. Create another output operator that takes a ``format_container`` and a ``vector`` and prints the vector on the given stream according to the example above.

(b) 

Generalize your second output operator by making it into a template parameterized on the container type. You can assume that there is an existing overload of the output operator for the element type (the type you get when dereferencing an iterator for the given container). After modifications, the code in listing 4 shall compile.

*Listing 4: Code after generalization*

```    
list<string> lst{”hi”, ”does”, ”this”, ”work?”}; forward_list<int> fl {3 ,65 ,1 ,8};
cout << format_container() << lst << ”\n”
     << format_container () << f l << endl ;
```

## Given Files:
```
#include <iostream>
#include <vector>
#include <list>
#include <forward_list>
using namespace std;
int main()
{
    // Part a
    vector<int> vec {2, 5, 1, 7, 10};
    cout << format_container() << vec << endl;
    cout << format_container('{') << vec << endl;

    // part b
    /*
    list<string> lst{"hi", "does", "this", "work?"};
    forward_list<int> fl{3,65,1,8};
    cout << format_container() << lst << "\n"
         << format_container() << fl << endl;
    */
}

```

## Facit:

```
#include <iostream>
#include <vector>
#include <list>
#include <forward_list>
using namespace std;

struct format_container
{
    format_container(char delim='[')
    {
        if (delim == '{')
        {
            start='{';
            end = '}';
        }
    }
    char start{'['}, end{']'};
    mutable ostream * os{};
};


template<typename Container>
ostream & operator<<(format_container const & cp, Container const & vals)
{
    *cp.os << cp.start;
    bool first {true};
    for ( auto && val : vals )
    {
        if ( first )
            first = false;
        else
            *cp.os << ", ";
        *cp.os << val;
    }
    return *cp.os << cp.end;
}

format_container const & operator<<(ostream & os, format_container const & cp)
{
    cp.os = &os;
    return cp;
}

int main()
{
    vector<int> vals {1,5,2,7,9};
    cout << format_container() << vals << endl;
    list<string> lst{"hi", "does", "this", "work?"};
    forward_list<int> fl{3,65,1,8};
    cout << format_container() << lst << "\n"
         << format_container('{') << fl << endl;
}
```
