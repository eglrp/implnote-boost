# Boost.TypeIndex

* lib: `boost/libs/type_index`
* repo: `boostorg/type_index`
* commit: `af61d6b8`, 2017-02-28

------
### Type index solution

#### Header

* `<boost/type_index.hpp>`

#### API

* `typedef xxxxx type_index`
  * `raw_name() const noexcept -> const char*`
  * `name() const noexcept -> const char*`
  * `pretty_name() const -> std::string`
  * `equal(const type_index&) const noexcept -> bool` - *Compared by `raw_name()`*
  * `before(const type_index&) const noexcept -> bool` - *Compared by `raw_name()`*
  * `hash_code() const noexcept -> std::size_t`
* `typedef type_index::type_info_t type_info`
* `type_id<T>() noexcept -> type_index`
* `type_id_with_cvr<T>() noexcept -> type_index`, which don't remove constenss and reference.
* `type_id_runtime<T>(const T&) noexcept -> type_index`;
* Operators: `==`, `!=`, `<`, `<=`, `>`, `>=`
* I/O: `<<`, `>>`
* `hash_value(const type_index&) noexcept -> std::size_t`
* Macro `BOOST_TYPE_INDEX_REGISTER_CLASS`, used to declare a hidden virtual function
  in user's polymorphic classes returning its `type_info`, used when RTTI is unusable.

#### Configurations

* Detected **RTTI** support, to select `std::type_index` or simulated **CTTI** solution.
* Macro `BOOST_TYPE_INDEX_FORCE_NO_RTTI_COMPATIBILITY`,
  force use **CTTI** even if **RTTI** is available.
* Macro `BOOST_TYPE_INDEX_USER_TYPEINDEX`, defined to be user-provided
  `type_index` implementation.

#### Extensibility

Derive from `type_index_facade`:

```c++
template <class Derived, class TypeInfo>
class type_index_facade { /* ... */ }
```

User may provide its own `type_info` class, and at least these members of `type_index`:
* `raw_name() const noexcept -> const char*`
* `type_info() const noexcept -> const type_info&`
* `static type_id<T>() noexcept -> type_index`
* `static type_id_with_cvr<T>() noexcept -> type_index`
* `static type_id_runtime<T>(const T&) noexcept -> type_index`

#### Implementation based on `std::type_info`

* Use `boost::core::demangled_name()` to get pretty name.
* Use a internal wrapper class to mark for `_with_cvr` case.
* Call hidden virtual function in `type_id_runtime` when RTTI is unavailable.

#### Implementation based on compile-time simulation

* `type_info` is an unusable type, `reinterpret_cast`ed from a C-string, which
  is extracted from a function's `BOOST_CURRENT_FUNCTION` (by default).
* `raw_name()` just cast back the `type_info` data to string.
* Customize macro `BOOST_TYPE_INDEX_CTTI_USER_DEFINED_PARSING` and
  `BOOST_TYPE_INDEX_FUNCTION_SIGNATURE` to override default compiler handling
  for extracting type from function name.

### `runtime_cast` (mimic `dynamic_cast`)

Header `<boost/type_index/runtime_cast.hpp>`

```c++
struct bad_runtime_cast : exception;

T runtime_cast<T,U>(U [const]* u) noexcept;
T [const]* runtime_cast<T,U>(U [const]* u) noexcept;

add_reference_t<[const] T> runtime_cast<T,U>(U [const]& u); // throws

std::shared_ptr<T> runtime_pointer_cast<T,U>(std::shared_ptr<U> const& u);
boost::shared_ptr<T> runtime_pointer_cast<T,U>(boost::shared_ptr<U> const& u);
```

* `BOOST_TYPE_INDEX_REGISTER_RUNTIME_CLASS` adds a recursive lookup virtual function.
  Arguments can be `BOOST_TYPE_INDEX_NO_BASE_CLASS` for nobase, and `(base1)(base2)...` for bases.
  It uses `type_id<T>()` internally.
* `runtime_cast` supports pointers, references
* `runtime_pointer_cast`, supports std/boost `shared_ptr`.
  It perform lookup for downcasting or sideway casting.

------
### Dependency

#### Boost.Config

* `<boost/config.hpp>`.

#### Boost.StaticAssert

* `<boost/static_assert.hpp>`.

#### Boost.ThrowException

* `<boost/throw_exception.hpp>`, when `std::type_info` based implementation is chosen.

#### Boost.Core

* `<boost/demangle.hpp>`, when `std::type_info` based implementation is chosen.
* `<boost/core/addressof.hpp>` - `runtime_cast`

#### Boost.TypeTraits

* `<boost/type_traits/*.hpp>`, for CV and Ref modifying.

#### Boost.MPL

* `<boost/mpl/if.hpp>`, `<boost/mpl/or.hpp>`, when `std::type_info` based implementation is chosen.
* `<boost/mpl/bool.hpp>`, when CTTI emulation implementation is chosen.

#### Boost.SmartPtr

* `<boost/smart_ptr/shared_ptr.hpp>` - `runtime_pointer_cast`.

#### Boost.Preprocessor

* `<boost/preprocessor/seq/for_each.hpp>` - `runtime_cast`

------
### Standard Facilities

* Standard Library: `<typeindex>` (C++11).
