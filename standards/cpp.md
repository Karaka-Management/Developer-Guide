# C/C++

The C/C++ code should focus on using simplicity over "modern solutions". This may often mean to heavily restrict the code. The following rule of thumb applies:

1. C99 and C++11 should be used where reasonable
2. C++ may be used in places where external libraries basically require C++

The reason for the strong focus on C is that we **personally** believe that C is simpler and easier to understand than the various abstractions provided by C++.

## Operating system support

C/C++ solutions should be valid on Windows 10+ and Linux.

## Performance

When writing code keep the following topics in mind:

* Branching / Branchless programming
* Instruction tables and their latency / throughput
* Cache Sizes
* Cache Line Size
* Cache Locality
* Cache Associativity
* Memory Bandwidth
* Memory Latency
* Prefetching
* Alignment / Packing
* Array of Structs vs Struct of Arrays
* SIMD
* Choosing correct data types
  * Data Type Sizes (e.g. 32 bit vs 64 bit)
  * Containers (e.g. arrays vs vectors)
  * Signed vs unsigned
* Threading

## Namespace

### use

Namespaces must never be globally used. This means for example `use namespace std;` is prohibited and functions from the standard namespace should be prefixed instead `std::`

## Data types

### Unsigned Integer

Be careful when you use unsigned and signed integers. When using unsigned integers the compiler may create additional instructions depending on the situation since it must support integer wrapping.

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
