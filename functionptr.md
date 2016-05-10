1. 指向一般函数的指针

函数指针的声明中就包括了函数的参数类型、顺序和返回值，只能把相匹配的函数地址赋值给函数指针。为了封装同类型的函数，可以把函数指针作为通用接口函数的参数，并通过函数指针来间接调用所封装的函数。
```cpp
//指向函数的指针
typedef int (*pFun)(int, int);


//使用时，指针前带*和不带*都一样，如下两种方法。
int Result(pFun fun, int a, int b)
{
return (*fun)(a, b);
}

int Result2(pFun fun, int a, int b)
{
return fun(a, b);
}
```

2. 指向类的成员函数的指针

类的静态成员函数采用与一般函数指针相同的调用方式，而受this指针的影响，类的非静态成员函数与一般函数指针是不兼容的。而且，不同类的this指针是不一样的，因此，指向不同类的非静态成员函数的指针也是不兼容的。指向类的非静态成员函数的指针，在声明时就需要添加类名。
下面是一个指向类的成员函数的指针的使用的例子，包括指向静态和非静态成员函数的指针的使用。
```cpp
class CA;
//指向类的非静态成员函数的指针
typedef int (CA::*pClassFun)(int, int);
//指向一般函数的指针*/
typedef int (*pGeneralFun)(int, int);
class CA {
    public:    
        int Min(int a, int b)    
        {         
            return a < b ? a : b;    
        }    
        static int Sum(int a, int b)    
        { return a + b;    }    
        /*类内部的接口函数，实现对类的非静态成员函数的封装*/    
        int Result(pClassFun fun, int a, int b)   
        {        
            return (this->*fun)(a, b);//fun前面必须带*，否则编译器会认为fun是具体的成员函数，而编译错误    
        }
  };
  /*类外部的接口函数，实现对类的非静态成员函数的封装*/
  int Result(CA* pA, pClassFun fun, int a, int b)
  {    
      return (pA->*fun)(a, b);//fun前面必须带*，否则编译器会认为fun是具体的成员函数，而编译错误
  }
  /*类外部的接口函数，实现对类的静态成员函数的封装*/
  int GeneralResult(pGeneralFun fun, int a, int b)
  {    
      return (*fun)(a, b);//fun前带*和不带*都一样。
  }
```
