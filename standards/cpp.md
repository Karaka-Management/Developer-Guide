# C/C++

The C/C++ code should focus on using simplicity over "modern solutions". This may often mean to heavily restrict the code. The following rule of thumb applies:

1. C24 should be used where reasonable (large parts of the cOMS framework)
2. C++ may be used in places where external libraries basically require C++

The reason for the strong focus on C is that we **personally** believe that C is simpler and easier to understand than the various abstractions provided by C++.

## Operating system support

C/C++ solutions must be valid on Windows 10+ and Linux.

## Namespace

### use

Namespaces must never be globally used. This means for example `use namespace std;` is prohibited and functions from the standard namespace should be prefixed instead `std::`

### Code structuring

It is encouraged to use C++ namespaces to structure code. In C programmers often use prefixes to more or less re-create namespaces. We consider this a hack and advocate for C++ namespaces.

```cpp
namespace Your::Name::Space {

}
```

## Templates

Don't use C++ templates.

## Allocation

Use memory arenas instead of over and over manually allocating memory.

However, if neccessary use C allocation methods for heap allocation.

## Functions

### C++ function

Don't use C++ standard library functions or C++ functions provided by other C++ header files unless you have to work with C++ types which is often required when working with third party libraries.

### Parameters

Generally, functions that take pointers to non-scalar types should modify the data instead of allocating new memory **IF** reasonable. This forces programmers to consciously create copies before passing data **IF** they need the original data. To indicate that a reference/pointer is not modified by a function define them as const!

We believe this approach provides a framework for better memory management and better performance in general.

Examples for this can be:

* Matrix multiplication with a scalar
* Sorting data (depends on sorting algorithm)
