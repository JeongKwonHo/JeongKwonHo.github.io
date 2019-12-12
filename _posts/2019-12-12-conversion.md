---
layout: post
title: "trivial type"
subtitle: "trivial type"
date: 2019-12-12 21:54:13 -0400
---

# trivial type

## trivial

사전적 정의 : 사소한, 하찮은, 하는일이 없는  
C++에서는 명시적으로 개발자가 구현하지 않고, 컴파일러가 자동으로 생성해주는 것을 의미
즉, 특별한 일을 하지 않겠다는 의미  

## Trivial default constructor

기본 생성자가 하는일이 없을때

```{.cpp}
#include <iostream>
#include <type_traits>

class Trivial {
    A() = default;
};

int main() {
    std::cout << std:::is_trivially_constructible<A>::value << std::endl;
}
```

### copy_type implementation C++11 style

```{.cpp}
template<typename T>
typename std::enable_if<std::is_trivially_copyable<T>::value>::type
copy_type(T* dst, T* src, int sz) {
    std::cout << "trivial type" << std::endl;
    memcpy(dst, src, sizeof(T)*sz);
}

template<typename T>
typename std::enable_if<!std::is_trvially_copyable<T>::value>::type
copy_type(T* dst, T* src, int sz) {
    std::cout << "is not trivial type" << std::endl;
    while(sz--) {
        new(dst) T(*src);
        ++dst, ++src;
    }
}
```

### copy_type implementation C++17 style

C++17부터는 type_traits에서 trvial 타입 helper template 지원

```{.cpp}
template<typename T>
inline constexpr bool is_triviallly_copyable_v = is_trivially_copyable<T>::value;
```

```{.cpp}

template<typename T>
std::enable_if_t<std::is_trivially_copyable_v<T>>
copy_type(T* dst, T* src, int sz) {
    std::cout << "trivial type" << std::endl;
    memcpy(dst, src, sizeof(T)*sz);
}

template<typename T>
std::enable_if<!std::is_trvially_copyable_t<T>
copy_type(T* dst, T* src, int sz) {
    std::cout << "is not trivial type" << std::endl;
    while(sz--) {
        new(dst) T(*src);
        ++dst, ++src;
    }
}
```
