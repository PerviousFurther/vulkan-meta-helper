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
template<typename T>
struct map;
template<::VkStructureType v>
struct type_constant;

#define DECORATE_VK_TYPE(s, t)\
template <> struct map<::s>{\
  static constexpr auto value{::t};\
};\
template <> struct map<type_constant<::t>>{\
  using type = s;\
};
#define BEGIN_DECORATE_VK_STRUCT(s) \
template <> struct traits<::s>{\
  static constexpr auto name{#s};

#define DECORATE_VK_STRUCT_MEMBER(s, t, m)\
  static constexpr auto name_##m{#m};\
  static constexpr auto type_##m{#t};
  
#define DECORATE_VK_STRUCT_MEMBER_ARRAY(s, t, size, m)\
  static constexpr auto name_##m{#m};\
  static constexpr auto type_##m{#t "[" #size "]"};

#define END_DECORATE_VK_STRUCT(s) }

#define BEGIN_DECORATE_VK_ENUM(s) \
inline constexpr auto make_##s(::std::string_view name__) {

#define DECORATE_VK_ENUM(s, n, v)\
  if(name__ == #n) return static_cast<::s>(v);

#define END_DECORATE_VK_ENUM(s)\
  return static_cast<::s>(0u);\
}

#include "vulkan_meta_helper.h"

...
using type = typename map<type_constant<::VK_STRUCTURE_TYPE_ACCELERATION_STRUCTURE_BUILD_GEOMETRY_INFO_KHR>>::type;
constexpr auto name0{traits<type>::name}; // "(name of type)"
constexpr auto name1{traits<type>::name_dstAccelerationStructure}; // "dstAccelerationStructure"
constexpr auto type0{traits<type>::type_dstAccelerationStructure}; // "VkAccelerationStructureKHR"

```

Then in following context. The library producer can 
use `traits<...>` on meta programming.

## What if the repository cannot meet your requirement?

The code is *generated* by an specified cplusplus program.
It's hard to say that the header is definitly complete and fit with all situations.
If u got an new idea and be liked to share with us. Feel free for commiting `issue`!