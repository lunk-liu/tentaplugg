---
title: "Kort och gott"
date: 2018-01-04
tags: ["c++", "plugg"]
draft: false
---

## rvalue
The name rule: If it has a name it’s an lvalue, otherwise it's a rvalue.

## Constructor delegation
Att en konstruktor utnytjar en annan konstruktor för att utföra sitt jobb. Tex ``Child(int x) : Parent{x} {};``, eller ``String() : String{ "" } {};``
