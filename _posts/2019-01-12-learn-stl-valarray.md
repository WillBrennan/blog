---
layout: post
title: Learn STL - What is std::valarray and how does it work?
tags: [C++, stl]
---

`valarray` is probably the least known standard library container, it uses [expression templates](https://en.wikibooks.org/wiki/More_C%2B%2B_Idioms/Expression-template) to optimize compute optimizations. This technique is the foundation of most numerical libraries such as Eigen, PyTorch, and TensorFlow.

[Expression templates](https://en.wikibooks.org/wiki/More_C%2B%2B_Idioms/Expression-template) improve the performance of compute operations as memory-bandwidth matters more than computational time. In naive libraries, there will be binary functions that perform operations such as addition or multiplication. This is equivalent of writing, 

```cpp
learn::vector<double> values(30000, 0.4);

for (auto& value: values) {
    value += 3.0;
}

for (auto& value: values) {
    value *= 2.1;
}

for (auto& value: values) {
    value *= value;
}
```
, this snippet loops through the data three times. But what does that mean? For each operation, the pre-fetcher has to get data from memory or cache, the operation is performed, and then its written back to memory. This is repeated 3 times. If this was written in a single loop; it only has to fetch the data once.

```cpp
learn::vector<double> values(30000, 0.4);

for (auto& value: values) {
    const auto tmp = 2.1 * (value + 3.0);
    value = tmp * tmp;
}
```
. As the size of `values` increases, the single loop version will tend to 3x quicker than the naive version. This is the problem that expression templates solve. For smaller vectors and matrices of <10 values the effect is negligible.

## How its used
To construct a `valarray` we can pass the value and number of elements, annoyingly in the opposite order as `vector`. Operations are performed with `valarray` as if it was a value. We can see this in the snippet below, 

```cpp
learn::valarray<double> values(0.4, 30000);

const auto tmp = 2.1 * value + 3.0;
values = tmp * tmp;
```
, this is equivalent to a single loop version. In some languages optimized for compute such as [Fortran](https://courses.physics.illinois.edu/phys466/sp2013/comp_info/array.html); array operations are performed this way by default. 

The `valarray` can then be accessed like other containers, 

```cpp
const double a = values[12];
```

## How it works
`valarray` and all operations on it rely on  and inherit from `detail::expression`. An `expression` consists of a `.size()` and a `operator[]` which is indexed from 0 to `.size()`, 

```cpp
namespace learn {
namespace detail {
template <typename ObjectT, typename ValueT>
class expression {
  public:
    using object = typename std::decay<ObjectT>::type;
    using value_type = ValueT;
    using size_type = std::size_t;

    value_type operator[](const size_type index) const {
        return static_cast<const object&>(*this)[index];
    }
    size_type size() const { return static_cast<const object&>(*this).size(); }

    operator object&() { return static_cast<object&>(*this); }
    operator const object&() { return static_cast<const object&>(*this); }
};

// more detail stuff...
} // namespace detail
```
. `expression` can be used for lazy evaluation and to construct expressions. To construct `operator+` for `valarray`, we use `binary_op` which takes a left-hand and right-hand expression. `binary_op` uses the [Curiously Recurring Template Pattern](https://en.wikibooks.org/wiki/More_C%2B%2B_Idioms/Curiously_Recurring_Template_Pattern) to inherit from `expression`. This lets binary operations be used together to form more complex expressions. We can see how `binary_op` can be used to construct `operator+` by passing `learn::plus` as a template type, 

```cpp
template <class LhsT, class RhsT, class Op>
class binary_op : public expression<binary_op<LhsT, RhsT, Op>, typename LhsT::value_type> {
  public:
    using Lhs = typename std::decay<LhsT>::type;
    using Rhs = typename std::decay<RhsT>::type;
    using size_type = std::size_t;
    using value_type = typename Lhs::value_type;

    static_assert(std::is_same<typename Lhs::value_type, typename Rhs::value_type>::value);

    binary_op(const LhsT& lhs, const RhsT& rhs) : lhs_(lhs), rhs_(rhs) {}

    value_type operator[](const size_type index) const { return Op()(lhs_[index], rhs_[index]); }
    size_type size() const { return lhs_.size(); }

  private:
    const Lhs& lhs_;
    const Rhs& rhs_;
};
}  // namespace detail

template <typename Lhs, typename Rhs>
auto operator+(const Lhs& lhs, const Rhs& rhs) {
    return detail::binary_op<Lhs, Rhs, plus<typename Lhs::value_type>>(lhs, rhs);
}
```
. The expression is then evaluated when `valarray` is constructed from `detail::expression`. The expression is evaluated in a single for-loop, avoiding the performance costs associated with naive operator overload implementations.

```cpp
template <typename ValueT>
class valarray : public detail::expression<valarray<ValueT>, ValueT> {
  public:
    using value_type = ValueT;
    using size_type = std::size_t;
    using reference = value_type&;
    using const_reference = const value_type&;
    // other type definitions

    // other constructors

    template <typename Expr>
    valarray(const detail::expression<Expr, ValueT>& expression) {
        data_.reserve(expression.size());

        for (size_type i = 0; i != expression.size(); ++i) {
            data_.emplace_back(expression[i]);
        }
    }

    size_type size() const { return data_.size(); }

    reference operator[](const size_type index) { return data_[index]; }
    const_reference operator[](const size_type index) const { return data_[index]; }

  private:
    using Storage = vector<value_type>;
    Storage data_;
};

} // namespace learn
```