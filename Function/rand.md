# 随机数（Random Number）

> `rand.cpp`文件内大部分为<u>静态函数</u>和<u>宏定义</u>，作用域为`cv`。

* RNG_NEXT(x)

> ```c++
> /*
>    Multiply-with-carry generator is used here:
>    temp = ( A*X(n) + carry )
>    X(n+1) = temp mod (2^32)
>    carry = temp / (2^32)
> */
> ```

```c++
#define RNG_NEXT(x) ((uint64)(unsigned)(x)*CV_RNG_COEFF + ((x)>>32))
```

注意：宏定义中，==小括号==的使用至关重要！

宏定义解释为：

```matlab
f(x) = A*x + carry
```

* DEF_RANDI_FUNC

```c++
#define DEF_RANDI_FUNC(suffix, type) \
static void randBits_##suffix(type* arr, int len, uint64* state, const Vec2i* p, void*, bool small_flag) \
{ randBits_(arr, len, state, p, small_flag); } \
\
static void randi_##suffix(type* arr, int len, uint64* state, const DivStruct* p, void*, bool) \
{ randi_(arr, len, state, p); }
```

```c++
DEF_RANDI_FUNC(8u, uchar)
DEF_RANDI_FUNC(8s, schar)
DEF_RANDI_FUNC(16u, ushort)
DEF_RANDI_FUNC(16s, short)
DEF_RANDI_FUNC(32s, int)
```

一个宏定义内声明两个函数，且传入的变量是存在冗余的。

## 伪随机数生成器

实现根本看不懂，跳过！

