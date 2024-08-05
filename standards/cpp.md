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
* Cost of abstractions
* atomics vs locking (mutex)
* Cache line sharing between CPU cores

### Branching / Branchless programming

Branched code

```c++
if (a < 50) {
    b += a;
}
```

Branchless code

```c++
b += (a < 50) * a;
```

### Instruction table latency

| Instruction | Latency | RThroughput |
|-------------|---------|:------------|
| `jmp`       | -       | 2           |
| `mov r, r`  | -       | 1/4         |
| `mov r, m`  | 4       | 1/2         |
| `mov m, r`  | 3       | 1           |
| `add`       | 1       | 1/3         |
| `cmp`       | 1       | 1/4         |
| `popcnt`    | 1       | 1/4         |
| `mul`       | 3       | 1           |
| `div`       | 13-28   | 13-28       |

https://www.agner.org/optimize/instruction_tables.pdf

### CPU stats

| CPU Category | Stat |
|--------------|---------|
| L1 Cache | 32 - 48 KB |
| L2 Cache | 2 - 4 MB |
| L3 Cache | 8 - 36 MB |
| L4 Cache | 0 - 128 MB |
| Clock speed | 3.5 - 6.2 Ghz  |
| Cache Line | 64 B |
| Page Size | 4 KB |

### Cache locality

Column wise traversal

```c++
void process_columns(int matrix[1000][1000]) {
    for (int col = 0; col < 1000; ++col) {
        for (int row = 0; row < 1000; ++row) {
            matrix[row][col] *= 2;
        }
    }
}
```

Row wise traversal

```c++
void process_rows(int matrix[1000][1000]) {
    for (int row = 0; row < 1000; ++row) {
        for (int col = 0; col < 1000; ++col) {
            matrix[row][col] *= 2;
        }
    }
}
```

### Data Padding

Wasting 6 bytes

```c++
struct Data {
    char a;
    int b;
    char c;
};
```

Wasting 2 bytes

```c++
struct Data {
    char a;
    char c;
    int b;
};
```

### SIMD

Performs every addition one after another:

```c++
void add_arrays(float* a, float* b, float* result, size_t size) {
    for (size_t i = 0; i < size; ++i) {
        result[i] = a[i] + b[i];
    }
}
```

> The code above may actually get optimized by the compiler because it is very simple

Performs 8 additions at the same time:

```c++
void add_arrays(float* a, float* b, float* result, size_t size) {
    size_t i = 0;
    for (; i < size - (size % 8); i += 8) {  // Process 8 elements at a time
        __m256 va = _mm256_loadu_ps(&a[i]);
        __m256 vb = _mm256_loadu_ps(&b[i]);
        __m256 vr = _mm256_add_ps(va, vb);
        _mm256_storeu_ps(&result[i], vr);
    }
    for (; i < size; ++i) {  // Handle the remainder
        result[i] = a[i] + b[i];
    }
}
```

### Cache line sharing between CPU cores

When working with multi-threading you may choose to use atomic variables and atomic operations to reduce the locking in your application. You may think that a variable value `a[0]` used by thread 1 on core 1 and a variable value `a[1]` used by thread 2 on core 2 will have no performance impact. However, this is wrong. Core 1 and core 2 both have different L1 and L2 caches BUT the CPU doesn't just load individual variables, it loads entire cache lines (e.g. 64 bytes). This means that if you define `int a[2]`, it has a high chance of being on the same cache line and therfore thread 1 and thread 2 both have to wait on each other when doing atomic writes.

## Namespace

### use

Namespaces must never be globally used. This means for example `use namespace std;` is prohibited and functions from the standard namespace should be prefixed instead `std::`

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
