# Boost.Sort

* lib: `boost/libs/sort`
* repo: `boostorg/sort`
* commit: `7b9e72ff`, 2017-04-01

------
### Spread Sort Algorithm

Header `<boost/sort/sort.hpp>` or `<boost/sort/spreadsort/spreadsort.hpp>`

```c++
RandomAccessIterator{RAIter}
  void spreadsort(RAIter first, RAIter last);
Range{R}
  void spreadsort(R& r);

RandomAccessIterator{RAIter}
  void integer_sort(RAIter first, RAIter last[, RShift][, Comp]);
Range{R}
  void integer_sort(R& r[, RShift][, Comp]);

RandomAccessIterator{RAIter}
  void float_sort(RAIter first, RAIter last[, RShift][, Comp]);
Range{R}
  void float_sort(R& r[, RShift][, Comp]);

Cast_type float_mem_cast<Cast_type>(const Data_Type&)
    requires sizeof(Cast_type) == sizeof(Data_Type) &&
              numeric_limits<Data_type>::is_iec559 && numeric_limits<Cast_type>::is_integer;

RandomAccessIterator{RAIter}
  void [reverse_]string_sort<UCharT=unsigned char>(RAIter first, RAIter last);
Range{R}
  void [reverse_]string_sort<UCharT=unsigned char>(R& r);

RandomAccessIterator{RAIter}
  void [reverse_]string_sort(RAIter first, RAIter last, Get_char, Get_length[, Comp]);
Range{R}
  void [reverse_]string_sort(R& r, Get_char, Get_length[, Comp]);
```

* `spreadsort` will forward to `float_sort`, `integer_sort`, or `string_sort` based on `value_type` of `RAIter`.
* `wstring` is supported only when `wchar_t` is 16-bit.
* `float_mem_cast` is provided to help implement `RShift` functor for floating point.
* `RShift`, `Comp`, and `Get_char`, `Get_length` allow user project to a data field of the `RAIter`'s value type.
* Configuration constants are in `<boost/sort/spreadsort/detail/constants.hpp>`, but is fixed now.

------
### Dependency

#### Boost.Config

* `<boost/config.hpp>`
* `<boost/cstdint.hpp>`

#### Boost.StaticAssert

* `<boost/static_assert.hpp>`

#### Boost.Serialization

* `<boost/serialization/static_warning.hpp>`

#### Boost.TypeTraits

* `<boost/type_traits.hpp>`

#### Boost.Core

* `<boost/utility/enable_if.hpp>`

#### Boost.Range

* `<boost/range/begin.hpp>`, `<boost/range/end.hpp>`

------
### Standard Facilities
