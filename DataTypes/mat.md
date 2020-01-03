# 大型数组类型

> ```c++
> typedef const _InputArray& InputArray;
> typedef InputArray InputArrayOfArrays;
> typedef const _OutputArray& OutputArray;
> typedef OutputArray OutputArrayOfArrays;
> typedef const _InputOutArray& InputOutputArray;
> typedef InputOutputArray InputOutputArrayOfArrays;
> ```

## MatAllocator

> Custom array allocator，可以看到这是一个==抽象类==。

## UMatData

> 这是一个结构体么？注释还说它可能不是一个单独的对象，所以有时没有构造和析构。不懂不懂。

## MatSize

```c++
struct CV_EXPORTS MatSize
{
	explicit MatSize(int* _p);
    int dims() const;
    Size operator() () const;
    const int& operator[] (int i) const;
    int& operator[] (int i);
    operator const int* () const;	// TODO: OpenCV 4.0: drop this
    bool operator == (const MatSize& sz) const;
    bool operator != (const MatSize& sz) const;
    
    int* p;
};
```

这个`指针p`是什么也没有解释。

## MatStep

```c++
struct CV_EXPORTS MatStep
{
	MatStep();
    explicit MatStep(size_t s);
    const size_t& operator[] (int i) const;
    size_t& operator[] (int i);
    operator size_t() const;	// 这。。。
    MatStep& operator = (size_t s);
    
    size_t* p;
    size_t buf[2];
protected:
    MatStep& operator = (const MatStep&);
};
```

运算符重载的形式好多啊。。

1. `Size operator() () const;`
    这是重载了`()`；
2. `operator size_t() const;`
    这是重载了`size_t()`，是个类型转换。

## Mat

> n维稠密数组。这个类的注释有很多，直接去源代码（mat.hpp）查看会更好一些。

```c++
class CV_EXPORTS Mat
{
public:
    Mat();
    // 有各种形式的构造函数。
    ...
        
	Mat row(int y) const;
    Mat col(int x) const;
    Mat rowRange(int startRow, int endRow) const;
    Mat rowRange(const Range& r) const;
    Mat colRange(int startcol, int endcol) const;
    Mat colRange(const Range& r) const;
    Mat diag(int d = 0) const;
    Mat clone() const CV_NODISCARD;
    ...
        
	int flags;
    int dims;
    int rows, cols;
    uchar* data;
    
    //! helper fields used in locateROI and adjustROI
    const uchar* datastart;
    const uchar* dataend;
    const uchar* datalimit;
    
    //! interaction with UMat
    UMatData* u;
    
    MatSize size;
    MatStep step;
};
```

* 根本摸不着头脑。跳过！