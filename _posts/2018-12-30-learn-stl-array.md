---
layout: post
title: Learn STL - How does std::array work?
tags: [C++, stl]
---
Array is deceptively simple, but how are its constructors and destructors implicitly declared? Why is array constexpr but vector isn't, and what is aggregate-initialization? Understanding Array requires a strong comprehension of these often over-looked language features.

Array is a container encapsulating fixed sized arrays and do not decay to T* unlike C arrays. It provides iterators, element-wise access, member types, everything you would expect from a standard library container. Its templated on the type, typename T, and the size, std::size_t N. But it has a few oddities; the constructor and assignment operator are implicitly declared, and how are begin() and end() defined when the size is zero?

# How its used
To construct an `array` we use [aggregate-initialization](https://en.cppreference.com/w/cpp/language/aggregate_initialization), but this leads to a few potential problems. We can initialize `values` with less than three elements. In this scenario, the unspecified elements are [value-initialized](https://en.cppreference.com/w/cpp/language/value_initialization), which in the case of `double` falls back to [zero-initialization](https://en.cppreference.com/w/cpp/language/zero_initialization). 

```cpp
template <typename Value, std::size_t N>
void print(const learn::array<Value, N>& values) {
    for (const auto& value: values) {
        std::cout << value << ' ';
    }

    std::cout << '\n';
}

learn::array<int, 3> values = {0, -2, 3};
print(values); // 0 -2 3

values = {3, 1};
print(values) // 3 1 0
```
This means that when we construct an `array` with no values, [value-initialization](https://en.cppreference.com/w/cpp/language/value_initialization) is performed on all elements.

```cpp 
learn::array<double, 3> values2;

print(values) // 0.0 0.0 0.0
```

We can then access the values with `get` or `operator[]`. It also has iterators which lets it be used in range-for loops. As `get` is templated on the index, it will fail at compile-time if its accessed out of bounds. Modern compilers and versions of `std::array` will do this with `operator[]`.

```cpp 
const auto& a = std::get<0>(values);
const auto& b = values[1];

for (const auto& c: values) {
    print(c);
}
```

## How it works
`array` is a struct, containing a C-style array. We can perform aggregate-initialization as `array` has, 

- no private or protected non-static data members
- no user-provided, inherited, or explicit constructors (explicitly defaulted or deleted constructors are allowed)
- no virtual, private, or protected (since C++17) base classes
- no virtual member functions
 
. We define internal C-style array so that it contains one element when the size is zero. This lets `array` have a size of zero and have defined behavior. This is in contrast to ISO C / C++ which does not allow arrays of size zero.

```cpp
template <typename Value, std::size_t Size>
struct array {
    using value_type = Value;
    
    // some type declerations... 
    // some member function declerations...

    value_type elems_[Size > 0 ? Size : 1];
};
```

By defining the size of `elems_` this way we can define the begin and end iterators, which are valid even when `Size == 0`.

```cpp
    // some member function declerations...

    constexpr iterator begin() noexcept { return iterator(elems_); }

    constexpr iterator end() { return iterator(elems_ + Size); }
    value_type elems_[Size > 0 ? Size : 1];
};
```