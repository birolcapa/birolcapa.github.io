---
layout: post
title:  "C++: List Initialization"
date:   2020-06-15 22:34:00 +0200
categories: software
---
### Disclaimer
These are my notes from "C++ Primer Fifth Edition (Stanley B. Lippman, Josée Lajoie, Barbara E. Moo)" while learning C++. I am just a student who repeats their words to understand C++ better.

### Initialization is not assignment
Initialization happens when we give a value while we are creating it. 
Assignment deletes an object's current value and replaces that value with the new one.

### List Initialization
4 different ways to define an int variable:

```cpp
int count = 0;
int count = {0};
int count{0};
int count(0);
```

The compiler will help us when we use list initialization:
```cpp
long double pi = 3.1415926536;

int x{pi}; // narrowing conversion required [1]
int y = {pi}; // narrowing conversion required [1]

int z(pi); // ok: but value will be truncated
int t = pi; // ok: but value will be truncated
```

[1] <https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#es46-avoid-lossy-narrowing-truncating-arithmetic-conversions>