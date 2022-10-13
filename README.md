# Puzzel Cheat Sheet

## Easy printing

```cpp
#include <iostream>
#include <sstream>
#include <string>
#include <string_view>

// base case for template recursion when one argument remains
template <class Arg1>
void print_impl(std::stringstream& ss, const std::string_view& name, const Arg1& arg1)
{
    ss << name << " = " << arg1;
}

// recursive variadic template for multiple arguments
template <class Arg1, class... Args>
void print_impl(
    std::stringstream& ss,
    const std::string_view& names,
    const Arg1& arg1,
    const Args&... args
) {
    const char* comma = strchr(names.data(), ',');
    size_t name_size(comma - names.data());
    print_impl(ss, std::string_view(names.data(), name_size), arg1);
    comma++; // Skip `,`.
    while (*comma == ' ') { comma++; }
    ss << ", ";
    print_impl(ss, comma, args...);
}

template <class... Args>
std::string vars_to_string(const std::string_view& names, const Args&... args) {
    std::stringstream ss;
    print_impl(ss, names, args...);
    return ss.str();
}

template <class Str>
void print_all(const Str& str) {
    std::cout << str;
}

template <class Str, class... Strs>
void print_all(const Str& str, const Strs&... strs) {
    std::cout << str << ", ";
    print_all(strs...);
}

#define PRINT_ARGS(...) \
    LOG(vars_to_string(#__VA_ARGS__, __VA_ARGS__)); \
    LOG("; ");
#define PRINT(...) \
    LOG(vars_to_string(#__VA_ARGS__, __VA_ARGS__)); \
    LOG(";\n");
/// Disable/Enable one of those.
// #define LOG(...) print_all(__VA_ARGS__);
#define LOG(...)
```
