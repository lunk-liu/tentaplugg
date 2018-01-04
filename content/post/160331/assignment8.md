---
title: "Programmeringsuppgift 8"
date: 2016-03-31
tags: ["c++", "programmeringsuppgifter", "160331"]
draft: false
---

Write your code in a file named assignment8.cc and submit your answer as Assignment #8.

In this assignment, you’re supposed to show good knowledge of the standard library. Any usage of standard iteration statements will give point deductions. In each step of your solution, make sure to use the algorithm most suited to solve the task – ``for_each`` is not a valid solution for all problems.


Here is a model competitive programming exercise for you to solve according to the algo- rithm presented after the problem text.

**Input:** two numbers N and T followed by N integer values separated by spaces (this range is called A), a blank line and T integers.
**Output:** For each integer t (1<=t<=N) from the T integers, print the sum of the t first integers in A.

**Example:**

    Input: 
    53 15236
    3 1 4
    Output:
    8
    1
    11


This task shall be solved in the following way. For each step, do nothing more and nothing less.

1. Read the two integers ``N`` and ``T`` using standard formatted input.
2. Create a vector, called input_values to store the range ``A``.
3. Read the `N`` integers and store them in ``input_values``.
4. Create a new vector ``sums``.
5. Calculate the accumulated sums from ``input_values`` so that index ``i`` in ``sums`` contains the sum of all values of ``input_values`` in the index range ``[0, i]``.
6. For each of the ``T`` remaining integers (it’s okay to read to end of input), print index ``t-1`` from ``sums`` to ``cout``.
    
Note: To get full marks on this exercise, you must make sure to prohibit unnecessary reallocations of the vectors during input and summation.

## Facit:

```
#include <iostream>
#include <iterator>
#include <algorithm>
using namespace std;

int main()
{
    int N,T;
    cin >> N >> T;                                                     // 1
    vector<int> input_values (N);                                      // 2
    copy_n(istream_iterator<int>{cin}, N, begin(input_values));        // 3
    vector<int> sums (N);                                              // 4
    partial_sum(begin(input_values), end(input_values), begin(sums));  // 5
    for_each(istream_iterator<int>{cin}, istream_iterator<int>{},
            [=](int t){cout << sums[t-1] << endl; });                  // 6

    return 0;
}

```
