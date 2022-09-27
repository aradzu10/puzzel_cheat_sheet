# puzzel_cheat_sheet

## Easy printing

```cpp
#define PRINT_VAR(var) #var " = " << var << ", "
#define PRINT DevNull()
// #define PRINT std::cout

struct DevNull{};
template <class T>
const DevNull& operator<<(const DevNull& dn, const T&) {
        return dn;
}
```
