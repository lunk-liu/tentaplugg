---
title: "Programmeringsuppgift 7"
date: 2016-06-02
tags: ["c++", "programmeringsuppgifter", "160602"]
draft: false
---

Copy the file assignment7.cc from given_files and make additions to your copy. Submit your solution as Assignment #7.

In this exercise, it’s extra important for you to use the standard library in a good way. To get full marks, use the correct algorithm for the given task – ``for_each`` is not a valid solution for all tasks and you won’t need any hand-rolled loops.

The given file has a simple function to test if a value is a prime number and a simple type alias called ``num_type``.
Your task is to create a program that generates a random number with its prime factors by following the algorithm below.

1. Create a ``vector<num_type>`` with space for 10 elements
2. Fill the vector with random values in the range ``[2, 75]``
3. Remove (with the help of the given ``is_prime`` function) the numbers that aren’t prime numbers
4. Sort the remaining values
5. Print the remaining numbers to standard output
6. Calculate the product of the remaining numbers
7. Print the product to standard output

## Given files:
```
using num_type = unsigned long long;

bool is_prime(num_type val, num_type divisor=2)
{
    if ( divisor*divisor > val )
    {
        return true;
    }
    if ( val % divisor == 0 )
    {
        return false;
    }
    return is_prime(val, ++divisor);
}

```

## Facit:
```
#include <functional>
#include <iostream>
#include <iterator>
#include <algorithm>
#include <random>
#include <vector>
using namespace std;

using num_type = unsigned long long;

constexpr bool is_prime(num_type val, num_type divisor=2)
{
    if ( divisor*divisor > val )
    {
        return true;
    }
    if ( val % divisor == 0 )
    {
        return false;
    }
    return is_prime(val, ++divisor);
}

int main()
{
    vector<num_type> vals(10);
    random_device rnd;
    uniform_int_distribution<num_type> dist{2,75};
    generate(begin(vals), end(vals),bind(dist,ref(rnd)));
    vals.erase(remove_if(begin(vals), end(vals),
                         [](num_type val){return !is_prime(val);}),
               end(vals));
    sort(begin(vals), end(vals));
    copy(begin(vals), end(vals), ostream_iterator<num_type>{cout," "});
    cout << accumulate(begin(vals),end(vals),1,multiplies<>{})<<endl;
}

```
