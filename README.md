# Vulkan Meta Helper

Single file on helping extand extra meta infomation 
between vulkan's struct and structure type.

## What is the purpose and functionality?

This repository is purpose on extend meta infomation 
structs in vulkan header, which can help u from *meta programming* on
`vulkan.h` by customing your extra meta information by 
single binary macro function `DEFINE_VK_META__(...)`.

*LIMITATION:* `vulkan.hpp` is not supported currently, but probably 
support in future.

## How to use it?

*Tips: The header will not automatically include `vulkan.h`.*

After including `vulkan.h`. It is neccessary to define 
`DEFINE_VK_META__(...)` before include `vulkan_meta_helper.h` 
*(Or just include the header without this macro, the helper header 
will make no effect on structs in vulkan)*.

Here is an simple sample:

```cpp
#include "vulkan\vulkan.h"
...
template<typename T>
struct traits;
#define DEFINE_VK_META__(sn, st)       \
template <> struct traits<::sn>{       \
  inline static constexpr c_str{#sn}   \
  inline static constexpr value{::st}; \
}
#include "vulkan_meta_helper.h"
...
```

Then in following context. The library producer can 
use `traits<...>` on meta programming.

## What if the repository cannot meet your requirement?

The code is *generated* by an specified cplusplus program.
It's hard to say that the header is definitly complete and fit with all situations.
If u got an new idea and be liked to share with us. Feel free for commiting `issue`!