# 基础数学运算

> 这些实现方式，真的好简洁。

[TOC]

## cvRound

> 浮点数转换为最相近的整数。

```c++
CV_INLINE 
int cvRound(double value)
{
    ...
	return (int)(value + (value >= 0 ? 0.5 : -0.5));
}
```

## cvFloor

> 浮点数转换为最相近的不大于自身的整数。

```c++
CV_INLINE
int cvFloor(double value)
{
    int i = (int)value;
    return i - (i > value);
}
```

## cvCeil

> 浮点数转换为最相近的不小于自身的整数。

```c++
CV_INLINE
int cvCeil(double value)
{
    int i = (int)value;
    return i + (i < value);
}
```

---

**后面这两个暂时看不懂。**

## cvIsNaN

> 判断一个浮点数是否是一个数字。（如果是则返回0，否则返回1。）
>
> NaN——not a number.

```c++
CV_INLINE
int cvIsNaN(double value)
{
    Cv64suf ieee754;
    ieee754.f = value;
    return ((unsigned)(ieee754.u >> 32) & 0x7fffffff) + ((unsigned)ieee754.u != 0) > 0x7ff00000;
}
```

其中Cv64suf是一个union：

```c++
typedef union Cv64suf
{
    int64 i;
    uint64 u;
    double f;
}
Cv64suf;
```

至于这个变量名为什么是`ieee754`，这是==《计算机基础》==的常识。

## cvIsInf

> 判断无穷数（正无穷大、负无穷大）。（如果是则返回1，否则返回0。）
>
> Inf——infinity

```c++
CV_INLINE
int cvIsInf(double value)
{
    Cv64suf ieee754;
    ieee754.f = value;
    return ((unsigned)(ieee754.u >> 32) & 0x7fffffff) == 0x7ff00000 && (unsigned)ieee754.u == 0;
}
```

