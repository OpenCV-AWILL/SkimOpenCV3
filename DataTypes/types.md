[toc]

# 基本数据类型

> `opencv2/core/types.hpp`
>
> * 命名空间：cv

## Complex

> 复数——实部`re`、虚部`im`。

```c++
template<typename _Tp> class Complex
{
public:
    Complex();
    Complex(_Tp _re, _Tp _im = 0);
    
    template<typename _Tp2>
    operator Complex<_Tp2> () const;
    Complex conj() const;
    
    _Tp re;
    _Tp im;
};

typedef Complex<float> Complexf;
typedef Complex<double> Complexd;
```

这个类进行了运算符重载，定义了abs、加、减、乘、除、等于、不等于。

## Point\_

> 2D点——x、y。这个类需要用向量理解。

```c++
template<typename _Tp> class Point_
{
public:
    typedef _Tp value_type;
    
    Point_();
    Point_(_Tp _x, _Tp _y);
    Point_(const Point_& pt);
    Point_(Point_&& pt) CV_NOEXCEPT;
    Point_(const Size_<_Tp>& sz);
    Point_(const Vec<_Tp, 2>& v);
    
    Point_& operator = (const Point_& pt);
    Point_& operator = (Point_&& pt) CV_NOEXCEPT;
    template<typename _Tp2>
    operator Point_<_Tp2> () const;
    operator Vec_<_Tp, 2> () const;
    
    _Tp dot(const Point_& pt) const;
    double ddot(const Point_& pt) const;
    double cross(const Point_& pt) const;
    bool inside(const Rect_<_Tp>& r) const;
    
    _Tp x;
    _Tp y;
};

typedef Point_<int> Point2i;
typedef Point_<int64> Point2l;
typedef Point_<float> Point2f;
typedef Point_<double> Point2d;
typedef Point2i Point;
```

这个类进行了运算符重载，定义了norm、加、减、乘、除、等于、不等于，包括了各种运算的情况。

## Point3\_

> 3D点——x、y、z。这个类需要用向量理解。

```c++
template<typename _Tp> class Point3_
{
public:
    typedef _Tp value_type;
    
    Point3_();
    Point3_(_Tp _x, _Tp _y, _Tp _z);
    Point3_(const Point3_& pt);
    Point3_(Point3_&& pt) CV_NOEXCEPT;
    explicit Point3_(const Point_<_Tp>& pt);
    Point3_(const Vec<_Tp, 3>& v);
    
    Point3_& operator = (const Point3_& pt);
    Point3_& operator = (Point3_&& pt) CV_NOEXCEPT;
    template<typename _Tp2> oprator Point3_<_Tp2> () const;
    operator Vec<_Tp, 3> () const;
    
    _Tp dot(const Point3_& pt) const;
    double ddot(const Point3_& pt) const;
    Point3_ cross(const Point3_& pt) const;
    
    _Tp x;
    _Tp y;
    _Tp z;
};

typedef Point3_<int> Point3i;
typedef Point3_<float> Point3f;
typedef Point3_<double> Point3d;
```

这个类进行了运算符重载，定义了norm、加、减、乘、除、等于、不等于，包括了各种运算的情况。

## Size\_

> 尺寸——宽、高。这个类用来指定矩形或图像的尺寸。

```c++
template<typename _Tp> class Size_
{
public:
    typedef _Tp value_type;
    
    Size_();
    Size_(_Tp _width, _Tp _height);
    Size_(const Size_& sz);
    Size_(Size_&& sz) CV_NOEXCEPT;
    Size_(const Point_<_Tp>& pt);
    
    Size_& operator = (const Size_& sz);
    Size_& operator = (Size_&& sz) CV_NOEXCEPT;
    
    _Tp area() const;
    double aspectRatio() const;
    bool empty() const;
    
    template<typename _Tp2>
    operator Size_<_Tp2> () const;
    
    _Tp width;
    _Tp height;
};

typedef Size_<int> Size2i;
typedef Size_<int64> Size2l;
typedef Size_<float> Size2f;
typedef Size_<double> Size2d;
typedef Size2i Size;
```

这个类进行了运算符重载，定义了加、减、乘、除、等于、不等于。

## Rect\_

> 矩形——x、y、width、height。
>
> OpenCV默认：
>
> * x和y是矩形的左上角的点
> * 左上边界包含，右下边界不包含

```c++
template<typename _Tp> class Rect_
{
public:
    typedef _Tp value_type;
    
    Rect_();
    Rect_(_Tp _x, _Tp _y, _Tp _width, _Tp _height);
    Rect_(const Rect_& r);
    Rect_(Rect_&& r) CV_NOEXCEPT;
    Rect_(const Point_<_Tp>& org, const Size_<_Tp>& sz);
    Rect_(const Point_<_Tp>& pt1, const Point_<_Tp>& pt2);
    
    Rect_& operator = (const Rect_& r);
    Rect_& operator = (Rect_&& r) CV_NOEXCEPT;
    
    Point_<_Tp> tl() const;
    Point_<_Tp> br() const;
    Size_<_Tp> size() const;
    _Tp area() cosnt;
    bool empty() const;
    bool contains(const Point_<_Tp>& pt) const;
    
    template<typename _Tp2>
    operator Rect_<_Tp2> () const;
    
    _Tp x;
    _Tp y;
    _Tp width;
    _Tp height;
};

typedef Rect_<int> Rect2i;
typedef Rect_<float> Rect2f;
typedef Rect_<double> Rect2d;
typedef Rect2i Rect;
```

构造函数（传入两个点，对这两个点没有任何要求）

```c++
template<typename _Tp> inline
Rect_<_Tp>::Rect_(const Point_<_Tp>& pt1, const Point_<_Tp>& pt2)
{
    x = std::min(pt1.x, pt2.x);
    y = std::min(pt1.y, pt2.y);
    width = std::max(pt1.x, pt2.x) - x;
    height = std::max(pt1.y, pt2.y) - y;
}
```

实现方法：

1. 由`pt1`和`pt2`得到`x`和`y`；
2. 然后求出`width`和`height`；

`empty()`、`contains()`、`tl()`、`br()`、`size()`这些实现都很简单。

这个类进行了运算符重载，定义了加、减、与、或、等于、不等于。

## RotatedRect

> 旋转矩形——中心点、边长（不是半边长）、角度。
>
> 注意：注释里面的`up-right rectangle`的意思是==直立的矩形==。

```c++
class CV_EXPORTS RotatedRect
{
public:
    RotatedRect();
    RotatedRect(const Point2f& center, const Size2f& size, float angle);
    RotatedRect(const Point2f& point1, cont Point2f& point2, const Point2f& point3);
    
    void points(Point2f pts[]) const;
    Rect boundingRect() const;
    Rect_<float> boundingRect2f() const;
    
    Point2f center;
    Size2f size;
    float angle;
};
```

* 为什么把比较复杂一点点的函数实现都放在了一个单独的文件（types.cpp）中呢？

构造函数（传入3个点，要求它们必须是顺序的，而不论是逆时针还是顺时针。）

```c++
RotatedRect::RotatedRect(const Point2f& _point1, const Point2f& _point2, const Point2f& _point3)
{
    ...
}
```

实现的方法：

1. 由`pt1`和`pt3`得到`center`；
2. 求出两个`相邻边的向量`；
3. 根据数学关系最终求出`size`、`angle`；

返回旋转矩形的4个`顶点`（顺序为左下、左上、右上、右下）

```c++
void RotatedRect::points(Point2f pt[]) const
{
    double _angle = angle * CV_PI / 180;
    float b = (float)cos(_angle) * 0.5f;
    float a = (float)sin(_angle) * 0.5f;
}
```

实现方法：

1. 由`angle`得到`sin(a)`和`cos(a)`；
2. 依次计算得到四个顶点坐标；（这毕业后，怎么求个这东西都这么麻烦。）

最小外接矩形的实现就很简单了，就是求最小，最大。

## Range

> `Range`类用于指定一个序列的连续子序列。它主要用于指定矩阵的行、列范围。
>
> 使用中可能要注意判断`all`的情况是否需特别处理。
>
> ```c++
> void MyFunction(..., const Range& r, ...)
> {
>     if(r == Range::all())
>     {
>         // process all the data
>     }
>     else
>     {
>         // process [r.start, r.end)
>     }
> }
> ```

```c++
class CV_EXPORTS Range
{
public:
    Range();
    Range(int _start, int _end);
    int size() const;
    bool empty() const;
    static Range all();
    
    int start, end;
};
```

`all()`的实现：

```c++
inline
Range Range::all()
{
    return Range(INT_MIN, INT_MAX);
}
```

这个类进行了运算符重载（`==`、`!=`、`!`、`&`、`&=`、`+`、`-`），没有重载`+=`、`-=`

## Scalar\_

> `Vec`的派生类，它是一个四元素向量。它还能与`CvScalar`相互转换。

```c++
template<typename _Tp> class Scalar_ : public Vec<_Tp, 4>
{
public:
    Scalar_();
    Scalar_(_Tp v0, _Tp v1, _Tp v2, _Tp v3 = 0);
    Scalar_(_Tp v0);
    
    Scalar_(const Scalar_& s);
    Scalar_(Scalar_&& s) CV_NOEXCEPT;
    
    Scalar_& operator = (const Scalar& s);
    Scalar_& operator = (Scalar_&& s) CV_NOEXCEPT;
    
    template<typename _Tp2, int cn>
    Scalar_(const Vec<_Tp2, cn>& v);
    
    //! returns a scalar with all elements set to v0.
    static Scalar_<_Tp> all(_Tp v0);
    
    //! conversion to another data type.
    template<typename _Tp2>
    operator Scalar_<_Tp2> () const;
    
    //! per-element product
    Scalar_<_Tp> mul(const Scalar_<_Tp>& a, double scale = 1) const;
    
    //! conjugate
    Scalar_<_Tp> conj() const;
    
    // return true if v1 == v2 == v3 == 0
    bool isReal() const;
};
```

这个类进行了运算符重载（`==`、`!=`、`+`、`-`、`*`、`/`、`+=`、`-=`、`*=`、`/=`），除法的操作种类会多一些。

## KeyPoint

> salient point detectors——特征点检测方法的类。这个类的instance存储一个由特征点检测方法（*如Harris corner detector、FAST、StarDetector、SURF、SIFT等*）得到的关键点。

```c++
class CV_EXPORTS_W_SIMPLE KeyPoint
{
public:
    CV_WRAP KeyPoint();
    KeyPoint(Point2f _pt, float _size, float _angle = -1, float _response = 0, int _octave = 0, int _class_id = -1);
    CV_WRAP KeyPoint(float x, float y, float _size, float _angle = -1, float _response = 0, int _octave = 0, int _class_id = -1);
    
    size_t hash() const;
    CV_WRAP static void convert(const std::vector<KeyPoint>& keyPoints, CV_OUT std::vector<Point2f>& points2f, const std::vector<int>& keyPointIndexes = std::vector<int>());
    CV_WRAP static void convert(const std::vector<Point2f>& points2f, CV_OUT std::vector<KeyPoint>& keyPoints, float size = 1, float response = 1, int octave = 0, int class_id = -1);
    
    CV_WRAP static float overlap(const KeyPoint& kp1, const KeyPoint& kp2);
    
    CV_WRAP_RW Point2f pt;
    CV_WRAP_RW float size;
    CV_WRAP_RW float angle;
    CV_WRAP_RW float response;
    CV_WRAP_RW int octave;
    CV_WRAP_RW int class_id;
};
```

* 这个类的`static`成员函数在==types.cpp==文件中。

convert函数无非是做一个类型转换。

```c++
void KeyPoint::convert(const std::vector<KeyPoint>& keyPoints, std::vector<Point2f>& points2f, const std::vector<int>& keyPointIndexes)
{
    CV_INSTRUMENT_REGION();
    
    if(keyPointIndexes.empty())
    {
        points2f.resize(keyPoints.size());
        for(size_t i = 0; i < keyPoints.size(); ++i)
        {
            points2f[i] = keyPoints[i].pt;
        }
    }
    else
    {
        points2f.resize(keyPointIndexes.size());
        for(size_t i = 0; i < keyPointIndexes.size(); ++i)
        {
            int idx = keyPointIndexes[i];
            if(idx >= 0)
            {
                points2f[i] = keyPoints[idx].pt;
            }
            else
            {
                CV_ERROR(CV_StsBadArg, "keyPointIndexes has element < 0. TODO: process this case");
                // points2f[i] = Point2f(-1, -1);
            }
        }
    }
}
```

```c++
void KeyPoint::convert(const std::vecctor<Point2f>& points2f, std::vector<KeyPoint>& keyPoints, float size, float response, int octave, int class_id)
{
    CV_INSTRUMENT_REGION();
    
    keyPoints.resize(points2f.size());
    for(size_t i = 0; i < points2f.size(); ++i)
    {
        keyPoints[i] = KeyPoint(points2f[i], size, -1, response, octave, class_id);
    }
}
```

overlap函数是做什么的？其实就是分了三种情况——包含、相交、相离。用到了==三角形的相关公式（高中）==，忘了。

## DMatch

> matching keypoint descriptors——用于匹配关键点描述符的简要类。

```c++
class CV_EXPORTS_W_SIMPLE DMatch
{
public:
    CV_WRAP DMatch();
    CV_WRAP DMatch(int _queryIdx, int _trainIdx, float _distance);
    CV_WRAP DMatch(int _queryIdx, int _trainIdx, int _imgIdx, float _distance);
    
    CV_PROP_RW int queryIdx;
    CV_PROP_RW int trainIdx;
    CV_PROP_RW int imgIdx;
    
    CV_PROP_RW float distance;
    
    bool operator < (const DMatch& m) const;
};
```

* 运算符重载（==是内联==）

```c++
inline
bool DMatch::operator < (const DMatch& m) const
{
    return distance < m.distance;
}
```

## TermCriteria

> Termination criteria——终止条件。

```c++
class CV_EXPORTS TermCriteria
{
public:
    enum Type
    {
        COUNT = 1,
        MAX_ITER = COUNT,
        EPS = 2
    };
    
    TermCriteria();
    TermCriteria(int type, int maxCount, double epsilon);
    
    inline bool isValid() const
    {
        const bool isCount = (type & COUNT) && maxCount > 0;
        const bool isEps = (type & EPS) && cvIsNaN(epsilon);
        return isCount || isEps;
    }
    
    int type;
    int maxCount;
    double epsilon;
};
```

*注： `isCount`这个表达式我就比较困惑。原因：不熟练==运算符的优先级==！背下来吧！多少次了！！！*

## Moments

> 这个东西现在看不懂。

