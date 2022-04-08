# C++

The c++ code should focus on using simplicity over "modern solutions". This may often mean to restrict the code on earlier c++ versions and sometimes even c code.

## Operating system support

C++ solutions must be valid on Windows 10 and Linux.

## Use of namespace

Namespaces must never be globally used. This means for example `use namespace std;` is prohibited and functions from the standard namespace should be prefixed instead `std::`

## Templates

Don't use c++ templates.