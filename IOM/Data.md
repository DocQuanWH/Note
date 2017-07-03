## C++ Data语意学

### 1. 空类的大小
定义一个空的类`CPoint3d`：
```C++
class CPoint3d {};

std::cout << sizeof(CPoint3d) << std::endl;  // VS2015: 1
```
空类`CPoint3d`中虽然没有定义任何数据，但编译器会强制的给类`CPoint3d`插入一个**char**（1 byte）型的数据。这种策略使得类`CPoint3d`的对象在内存中都有自己独立的地址。
```C++
CPoint3d Point1;
CPoint3d Point2;

std::cout << "Point1: " << &Point1 << std::endl;  // VS2015: 0x0058f833
std::cout << "Point2: " << &Point2 << std::endl;  // VS2015: 0x0058f827
```

### 2. 