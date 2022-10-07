# Puzzel Cheat Sheet

## Easy printing

```cpp
#include <string_view>

// base case for template recursion when one argument remains
template <typename Arg1>
void print_impl(const std::string_view& name, const Arg1& arg1)
{
    std::cout << name << " = " << arg1;
}

// recursive variadic template for multiple arguments
template <typename Arg1, typename... Args>
void print_impl(const std::string_view& names, const Arg1& arg1, const Args&... args)
{
    const char* comma = strchr(names.data(), ',');
    size_t name_size(comma - names.data());
    print_impl(std::string_view(names.data(), name_size), arg1);
    comma++; // Skip `,`.
    while (*comma == ' ') { comma++; }
    std::cout << ", ";
    print_impl(comma, args...);
}

#define PRINT(...) print_impl(#__VA_ARGS__, __VA_ARGS__); std::cout << ";\n";
// #define PRINT(...)
```
