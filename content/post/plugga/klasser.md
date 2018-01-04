---
title: "Konstruktorer osv"
date: 2018-01-04
tags: ["c++", "plugg"]
draft: false
---

```
class String
{
public:
using size_type = std::size_t;      // nested type (alias declaration)
String() = default;                 // default constructor, compiler generated
String(const String&);              // copy constructor
String(String&&) noexcept ;         // move constructor
String(const char*);                // type converting constructor, from C string
~String();                          // destructor

String& operator=(const String&) &;     // copy assignment operator, ref-qualifier (lvalues only)
String& operator=(String&&) & noexcept; // move assignment operator
String& operator=(const char*) &;   // type converting assignment operator

size_type length() const;           // const member function – accessor function
// ...
};

```

- default constructor: initializes a new object without any arguments.
- copy constructor: initializes a new object from an existing object of the same type.
- move constructor: typically used if the source is a temporary object of the same type.
- copy assignment: assigns from an object of the same type.
- move assignment: typically used if the right hand side is a temporary object of the same type.
- destructor: cleans up when an object cease to exist, typically releasing resources.

```
struct Cool {
    Cool& operator=(Cool&&);     // move assignment operator
    Cool(Cool&& other);          // move assignment constructor

}

Cool icecream = move(chocolate); // uses the move assignment operator
Cool s1 = Cool{ "C++" };         // uses the move assignment constructor. temporary object (rvalue).
Cool s2{ std::move(s1) };        // utility function move() converts s1 (lvalue) to Cool&& (rvalue)

```



## rvalue
The name rule: If it has a name it’s an lvalue, otherwise it's a rvalue.

## Constructor delegation
Att en konstruktor utnytjar en annan konstruktor för att utföra sitt jobb. Tex ``Child(int x) : Parent{x} {};``, eller ``String() : String{ "" } {};``

## Delete some constructors

Some objects, e.g. stream objects, are not suitable to copy.

- copy construction and copy assignment can be disallowed
- declare **delete** to remove a special member functions,
- **default** if you want the compiler to generate

```
public :
X() = default;          // default initialization allowed
X(const X&) = delete;    //no copy constructionX&operator=(const
X&) = delete;           // no copy assignment 
```

- if, e.g., the compiler generated copy constructor is fine, but you want to restrict it for internal use only,default it asprotected

```
protected:
    X(const X&) = default;
```