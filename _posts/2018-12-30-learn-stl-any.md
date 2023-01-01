---
layout: post
title: Learn STL - How does std::any work?
tags: [C++, stl]
---
Sometimes in C++ you really need to break out of the type-system. This can be done using `std::any`! It can store any value; but how does it work and how can it be used? We implement it in [150 lines in learn-stl!](https://github.com/WillBrennan/learn_stl/blob/master/learn_stl/any.h)

We will answer the following questions; 

* Whats the runtime overhead in using `std::any`?
* How does it know what type its storing? 
* Would would you need `std::any`?


## How its used?
`any` is a type-safe container that can store any value type at run-time and it can even be empty! Its not templated like [variant](/2018/12/30/learn-stl-variant.html). 

```cpp
learn::any any_value = "hello world";
any_value = 12.345f;
any_value = 23.45;
any_value = 12;
```

It can be accessed using `any_cast`. Several variants of `any_cast` exist, allowing it to be accessed in different ways. Each has their own performance trade-off; you might want to avoid copying or throwing exceptions!

```cpp
// Cast 1- return copy of internal value
const int my_value0 = learn::any_cast<int>(any_value);

// Cast 1 - throws a std::bad_any_cast as type is bad
const double my_value1 = learn::any_cast<double>(any_value);

// Cast 2 - returns a nullptr if the type is bad! 
const int* my_ptr = learn::any_cast<int>(&any_value);

// Cast 3 - moves the value out of the any container
const int my_value2 = learn::any_cast<int>(learn::move(any_value));
```

If you want to check the type thats stored; you can use the `type()` member function. 

```cpp
if (!any_value.has_value()) {
    return -1;
}

if (any_value.type() == typeid(int)) {
    return learn::any_cast<int>(any_value);
}
```

But why would you want to use this? This replaces the `void*` pattern thats common in older code bases; `std::any` adds type safety. This is used to limit and control the number of dependencies in larger codebases and systems. This is commonly found in message passing / file parsing when handlers should be agnostic to what they are acting on. 

This should be a class of last resort; `optional` or `variant` should be used instead.


## How it works
But how does `any` do this? Internally, `any` has a `ContainerInterface` and a templated `Container`, which is templated on the stored value type. 

```cpp
struct ContainerInterface {
    using Ptr = ErasedPtr<ContainerInterface>;

    virtual ~ContainerInterface() = default;
    virtual const std::type_info& type() const = 0;

    virtual Ptr copy() const = 0;
};

template <typename ObjectT>
struct Container : ContainerInterface {
    using Object = std::decay_t<ObjectT>;

    template <typename... Args>
    static Ptr make(Args&&... args) {
        return Ptr(new Container(forward<Args>(args)...));
    }

    explicit Container(const Object& _data) : data(_data) {}
    explicit Container(Object&& _data) : data(forward<Object>(_data)) {}
    Object data;

    const std::type_info& type() const final { return typeid(data); }

    Ptr copy() const final { return make(data); }
};
```
We store a `unique_ptr` to a `Container` through `ContainerInterface`, allowing the `ContainerInterface` pointer to store a `Container` for any type. Most standard library implementations use a small-value optimization (SVO) to improve performance. This can be seen in the [learn_stl implementation of any](https://github.com/WillBrennan/learn_stl/blob/master/learn_stl/any.h#L97)

To access the stored value, we perform a `dynamic_cast` to cast down the inheritance hierarchy. If the cast fails, then it returns a nullptr. Below you can see how Cast 2 is done.

```cpp
template <typename Object>
const Object* any_cast(const any* value) {
    auto container_ptr = dynamic_cast<const any::Container<Object>*>(value->container_.get());

    return container_ptr ? addressof(container_ptr->data) : nullptr;
}
```

This function can be used to implement the other `any_cast` functions.