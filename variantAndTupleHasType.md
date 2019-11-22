# Checking if a tuple/variant have a particular type

```cpp
// template interface. placeholder is just to make a paramter
// we can specialize on for the specialization
template <typename T, typename VariantPlaceholder>
struct has_type;

// this speecializes on std variant with no types. base/terminating case
template <typename T>
struct has_type<T, std::variant<>> : std::false_type {};

// this is the case where the first type is not T. in that case 
// inherit from the next specialization and forward the parameter pack.
template <typename T, typename U, typename... Ts>
struct has_type<T, std::variant<U, Ts...>> : has_type<T, std::variant<Ts...>> {};

// if the above template inherited from this one (ie this one matches and is specialized)
// then it will be the final one chosen and be true
template <typename T, typename... Ts>
struct has_type<T, std::variant<T, Ts...>> : std::true_type {};
```
from https://stackoverflow.com/a/25958302/2089130

In fact the variant can be swapped out here for any template class that has 1 or more types as parameters.
It could be used to check if a vector is using a particular allocator. 

It can be used as an input for std conditional or enable_if
