---
layout: post
title: Learn STL - How does std::tuple work?
tags: [C++, stl]
---
The best way to know something in detail is to implement it.  We implement a working example of `tuple` in 100 lines in [learn_stl/tuple.h](https://github.com/WillBrennan/learn_stl/blob/master/learn_stl/tuple.h). How does `tuple` store multiple values and how does `get` work using both the index and type? Whats its run-time overhead?

`tuple` is a vocabulary container consisting of a collection of heterogenous types, like `pair`, but with any number of types. `tuple` is one of the most interesting containers in the standard library as its implemented using techniques that are very different from most peoples day-to-day experience with C++. `tuple` is also the only standard library container to accept l-value references.

We will answer the following questions;

- What is the overhead of `get<>` with a tuple?
- How does it handle multiple types in a variadic template?
- How can `get` use both index and type?

## How its used
To construct a `tuple` we use [list-initialization](https://en.cppreference.com/w/cpp/language/list_initialization), prior to C++17 we had to use `make_tuple`. As shown below, you can use [structured bindings](https://en.cppreference.com/w/cpp/language/structured_binding) to unpack a tuple into its sub-objects, 

```cpp
learn::tuple<int, double, char, double> myFunction() {
    return {1, 3.04, 'a', -3.232};
}

int main() {
    const auto [a, b, c, d] = myFunction();
}
```
. Prior to C++17 results were extracted from a tuple using `tie` or `get`. `get` is used to extract single values by either its index or type. If multiple values exist of the same type then `get` returns the first value of that type. `tie` can be used to extract all values at once, it does this by forming a tuple of l-value references, we can then use `tuple::operator=` to assign the value of each of the elements in the tuple produced by `tie`. To ignore an element while using tie, `ignore` can be used,

```cpp
int main() {
    const auto result = myFunction();

    const auto& a = learn::get<0>(result);
    const auto& b = learn::get<double>(result);

    int x;
    double y;
    char z;

    learn::tie(x, y, z, learn::ignore) = result;
}
```
.  `tie` can also be used to perform lexicographical comparisons between structs, its common to see it used like, 

```cpp
struct Thing {
    int a;
    float b;
    char c;

    bool operator==(const Thing& other) const {
        return learn::tie(a, b, c) == learn::tie(other.a, other.b, other.c);
    }
};
```
. Since C++20, the spaceship `<=>` operator should be used.

## How it works

How does `tuple` perform all this magic? The answer is a lot of variadic templates! Each element in a tuple is contained by a `tuple_leaf` which is templated on the element type and index, this means that each `tuple_leaf` is unique within a `tuple`, 

```cpp
namespace learn {
namespace detail {

template <size_t I, typename Type>
struct tuple_leaf {
  public:
    using type = Type;
    explicit constexpr tuple_leaf(const type& value) : value_(value) {}
    explicit constexpr tuple_leaf(Type&& value) : value_(move(value)) {}

    Type value_;
};
```
. To construct a tuple from the leaves we need to produce a compile-time sequence of integers the same size as the number of values `tuple` is templated on, and then convert it to a sequence of integers and values to template `tuple_leaf`. To start, we create our `tuple` with a variadic template, `typename... Types`, to get the number of types in our variadic template we call `sizeof...(Types)`. This lets us crate a compile-time sequence of integers using `make_index_sequence<sizeof...(Types)>::type`. We pass these onto `detail::tuple` which expands the index sequence and type sequence into our `detail::tuple_leaf`s, `: tuple_leaf<Indices, Types>(elements)...`. The code is simpler than it sounds,

```cpp
template <size_t... Indices, typename... Types>
struct tuple<index_sequence<Indices...>, Types...> : tuple_leaf<Indices, Types>... {
    explicit constexpr tuple(const Types&... elements) : tuple_leaf<Indices, Types>(elements)... {}
    explicit constexpr tuple(Types&&... elements)
        : tuple_leaf<Indices, Types>(forward<Types>(elements))... {}
};
} // namespace detail

template <typename... Types>
class tuple : public detail::tuple<typename make_index_sequence<sizeof...(Types)>::type, Types...> {
  public:
    explicit constexpr tuple(const Types&... elements) : TupleImpl(elements...) {}
    explicit constexpr tuple(Types&&... elements) : TupleImpl(forward<Types>(elements)...) {}

  private:
    using TupleImpl = detail::tuple<typename make_index_sequence<sizeof...(Types)>::type, Types...>;
};
} // namespace learn
```
. 

This is the simple part of `tuple`! The next question is, how do we access the values? 

As `tuple` only consists of the values in order *(and lots of templating magic)*, structured-bindings implicitly works. But how about `tie` and `get`? 

`tie` is relatively simple as `tuple` accepts l-value references; this function constructs a `tuple` of references and the `tuple::operator=` is used to assign each value. To implement `ignore` we have a class with a templated copy-assignment operator that accepts any value. This way it *ignores* whatever is assigned!

```cpp
namespace learn {
namespace detail {
struct ignore_t {
    template <typename T>
    const ignore_t& operator=(const T&) const { return *this; }
};
} // namespace detail 
const detail::ignore_t ignore;
 
template <typename... Args>
auto tie(Args&... args) {
    return tuple<Args&...>(args...);
}
} // namespace learn 
```
. `get` is a lot trickier! It works by casting a `tuple` to the required `tuple_leaf` type, and then returning the value. The hard bit is working out the type at a given index, or the index of a given type. This is done with `tuple_element_t`;

```cpp
template <size_t Index, typename... Types>
constexpr auto& get(tuple<Types...>& data) noexcept {
    using Tuple = tuple<Types...>;
    using Type = tuple_element_t<Index, Tuple>;
    using Element = detail::tuple_leaf<Index, Type>;

    Element& base = data;
    return base.value_;
};
```
. The index version of `tuple_element_t` is done using a recursive template called `type_at_index` which recursively goes through the tuple until a template specialization is hit, which gives the correct type definition for that type index. The opposite is done for getting an index from a type. This leads to the behavior of `get` returning the first element of a type when multiple elements have the same type.

```cpp
namespace detail {
template <size_t I, typename Head, typename... Tail>
struct type_at_index {
    using type = typename type_at_index<I - 1, Tail...>::type;
};

template <typename Head, typename... Tail>
struct type_at_index<0, Head, Tail...> {
    using type = Head;
};

template <size_t I, typename... Types>
using type_at_index_t = typename type_at_index<I, Types...>::type;
} // namespace detail

template <size_t I, typename Type>
class tuple_element {
  public:
    using type = Type;
};

template <size_t I, typename... Types>
class tuple_element<I, tuple<Types...>> {
  public:
    using type = detail::type_at_index_t<I, Types...>;
};

template <size_t I, typename Tuple>
using tuple_element_t = typename tuple_element<I, Tuple>::type;
```