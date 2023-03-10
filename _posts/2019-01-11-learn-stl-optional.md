---
layout: post
title: Learn STL - How does std::optional work?
tags: [C++, stl]
---
`optional` is a vocab type that lets you "optional" store a value; and is often when a function might fail to return a value. Its designed as a replacement to `unique_ptr` but to avoid the slow dynamic allocation! 

This is one of the simplest containers, `optional` has many similar properties to `unique_ptr` such as implicit conversion to bool, and `operator->` and `operator*` to access the contained value.

## How its used
`optional` is often used as a return-type; when the function might fail to return results but isn't exceptional enough for a costly exception to be thrown.

```cpp
struct Result {
    int sum;
    double other;
};

learn::optional<Result> myFunction(int value) {
    if (value < 10) {
        return Result{value + 10, 2.5 * value};
    }

    return {};
}

int main() {
    if (const auto maybeResult = myFunction(4)) {
        const Result& result = maybeResult.value();
        std::cout "my results: " << result.sum << ' ' << result.other << '\n';
    } else {
        std::cout "my function didnt produce a result\n";
    }
}
```

In C++23 `expected` is likely to be introduced and will have additional memeber functions to behave like a monad. 

## How it works
So how can `optional` be empty and not require any dynamic allocation? This trick is performed using a union type to form an `optional_storage` struct, this struct can contain a value `T` or a placeholder `dummy_t` to represent the empty state, 

```cpp
namespace detail {
struct dummy_t {};

template <typename T>
union optional_storage {
    static_assert(std::is_trivially_destructible<T>::value, "");

    dummy_t dummy_;
    T value_;

    constexpr optional_storage()  // null-state ctor
        : dummy_{} {}

    constexpr optional_storage(T const& v)  // value ctor
        : value_{v} {}

    ~optional_storage() = default;  // trivial dtor
};
}  // namespace detail
```
. As `dummy_t` is an empty class; it means that `sizeof(optional_storage) == sizeof(T)`. Unfortunately, `sizeof(optional<T>) > sizeof(T)` as `optional_storage` does not tell us what value its storing, `optional` holds this information as a bool. This lets `optional` perform the same operations as `unique_ptr` without dynamic allocation, 

```cpp
template <typename Object>
class optional {
  public:
    constexpr optional() : has_value_(false) {}

  private:
    using Storage = detail::optional_storage<Object>;
    bool has_value_;
    Storage storage_;
};
```
. Finally, we can see that the `optional` has a default constructor and relies on the default constructor of `optional_storage`. As such, the order of values within `optional_storage` is important, `dummy_t` must be before `T` so that `optional_storage` default constructs `dummy_t`.

The full implementation can be found in [learn-stl optional](https://github.com/WillBrennan/learn_stl/blob/master/learn_stl/optional.h).