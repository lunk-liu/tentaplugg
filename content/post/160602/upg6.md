---
title: "Fråga 6"
date: 2016-06-02
tags: ["c++", "programmeringsuppgifter", "160602"]
draft: false
---

Copy the file __assignment6.cc__ from __given_files__ and make additions to your copy. Submit your solution as __Assignment #6__.
There is a built-in problem with some of the integral types, __int__ in particular, in that we get undefined beavior when going outside the range of the type. Your task in this assignment is to create a type ``Modular`` that is templated on the internal storage type and the value range (given as two template non-type parameters). The value range shall be defaulted as the default value range for the given type given by ``numeric_limits`` from the ``<limits>`` header. Your class shall support the following operations:

- Explicit constructor for the underlying type. If the value given is outside the current value range, the value shall be set to the lowest value.
- Assignment from underlying type. If the value is outside the value range, the assign- ment shall not modify the object but throw an ```std::domain_error`` exception.
- Implicit conversion to the underlying type.
- Incrementing operators (both postfix and prefix versions) to step the value by one.
- Addition with another Modular object, possibly having another value range. The value range of the result shall be the same as the left hand side object.
The given file has a main program that tests these operations.

## given files:
```
#include <stdexcept>
#include <iostream>
using namespace std;

int main()
{
    Modular<int, 2, 10> m{3};
    cout << m << endl;
    try
    {
        m = 1;
    }
    catch ( std::domain_error const & e)
    {
        cout << e.what() << endl;
    }
    Modular<int, 2,10> m2 {5};
    m = m + m2;
    cout << m << endl;
    m = m + Modular<int,-11,-2>{-10};
    cout << "Should be 7: " << m << endl;
    cout << "Should print 7 9: " << m++ << " ";
    cout << ++m;
    Modular<char,'a','c'> mc{'b'};
    ++mc;
    cout << "\nShould print a: " << ++mc << endl;
}
```

## Facit:

```
#include <iostream>
#include <limits>
using namespace std;

template <typename T, T min=numeric_limits<T>::min(), T max=numeric_limits<T>::max()>
class Modular
{
public:
    explicit Modular(T n) 
      : val{n}
    {
        if ( n < min || n > max )
            val = min; 
    }
    Modular & operator=(T const & n)
    {
        if ( n < min || n > max)
           throw domain_error{"Value out of range"};
        val = n;
        return *this;
    }
    operator T() const
    {
        return val;
    }

    template <T o_min, T o_max>
        Modular<T,min,max> operator+(Modular<T,o_min,o_max> const & rhs) const
        {
            Modular<T,min,max> m{adjust(val+rhs)};
            return m;
        }
    
    Modular<T,min,max> & operator++()
    {
        if ( val == max )
            val = min;
        else
            ++val;
        return *this;
    }
    Modular<T,min,max> operator++(int)
    {
        auto tmp = *this;
        ++*this;
        return tmp;
    }
private:
    T adjust(T n) const
    {
        if ( n < max && n > min )
        {
            return n;
        }
        else if (n > max )
        {
            return min + n - max - 1;
        }

        return max + n - min + 1;
    }
    T val;
};

int main()
{
    Modular<int, 2, 10> m{3};
    cout << m << endl;
    try
    {
        m = 1;
    }
    catch ( std::domain_error const & e)
    {
        cout << e.what() << endl;
    }
    Modular<int, 2,10> m2 {5};
    m = m + m2;
    cout << m << endl;
    m = m + Modular<int,-11,-2>{-10};
    cout << "Should be 7: " << m << endl;
    cout << "Should print 7 9: " << m++ << " ";
    cout << ++m;
    Modular<char,'a','c'> mc{'b'};
    ++mc;
    cout << "\nShould print a: " << ++mc << endl;
}


```