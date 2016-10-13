### C++学习注意事项
-----------

C++11不允许字符串转化为char\*,如Ivor Horton的Beginning Visual C++ 2013中说
>You can store the address of a string literal in a pointer that is not const. This was removed from C++ by the C++ 11 standard but is still allowed by the Visual C++ compiler so as not to break existing code. Storing the address of a string literal in a non-const pointer is dangerous; you should not do it.   

意思就是只能用const char*指向一个常量字符串. 

```
#include <iostream>
using namespace std;
int main(){
    int n = 3;
    cout<<"hello world"<<endl<<n<<endl;
    const char* str = "hello world";
    cout<<str<<endl;
    return 0;
}
```
```
warning: ISO C++11 does not allow conversion from string literal to 'char *' [-Wwritable-strings]
        char* str = "hello world";
                    ^
1 warning generated.
```     

--------
###### C++ 命名空间
在文件中可以定义全局变量(global variable),它的作用域是整个程序。如果在文件A中定义了一个变量a,`int a=3;`
在文件B中可以再定义一个变量a,` int a=5; `在分别对文件A和文件B进行编译时不会有问题。但是，如果一个程序包括文件A和文件B，那么在进行连接时,会报告出错，因为在同一个程序中有两个同名的变量，认为是对变量的重复定义.可以通过extern声明同一程序中的两个文件中的同名变量是同一个变量。如果在文件B中有以下声明:
```C++      
extern int a;
```
表示文件B中的变量a是在其他文件中已定义的变量。由于有此声明，在程序编译和连接后，文件A的变量a的作用域扩展到了文件B。如果在文件B中不再对a赋值，则在文件B中用以下语句输出的是文件A中变量a的值：` cout<<a; ` //得到a的值为3.  

*** 命名空间：实际上就是一个由程序设计者命名的内存区域，程序设计者可以根据需要指定一些有名字的空间域，把一些全局实体分别放在各个命名空间中，从而与其他全局实体分隔开来. ***  

[CSDN博客详细讲解C++ 命名空间](http://blog.csdn.net/liufei_learning/article/details/5391334)

--------
C++中的this功能上等于java中的this,不过C++中的this指针指向当前对象的地址，只能用->操作符引用对象成员.例如:  
```C++
void Date::setMonth( int mn ) 
    { 
     month = mn; // 这三句是等价的 
     this->month = mn; 
     (*this).month = mn; 
    }
``` 
-------
######C++创建对象的三种方式以及注意事项: [博客地址](http://blog.csdn.net/azhexg/article/details/14225545)  
C++中有三种创建对象的方法  
```C++
    A a(1);  //栈中分配
    A b = A(1);  //栈中分配
    A* c = new A(1);  //堆中分配
　　delete c;
```
    

第一种和第二种没什么区别，一个隐式调用，一个显式调用，两者都是在进程虚拟地址空间中的栈中分配内存，而第三种使用了new，在堆中分配了内存，而栈中内存的分配和释放是由系统管理，而堆中内存的分配和释放必须由程序员手动释放。采用第三种方式时，必须注意一下几点问题:   

* new创建类对象需要指针接收，一处初始化，多处使用
* <span style="color: red">new创建类对象使用完需delete销毁</span>
* new创建对象直接使用堆空间，而局部不用new定义类对象则使用栈空间
* <span style="color: red">new对象指针用途广泛，比如作为函数返回值、函数参数等</span> //*** 因为是对象是在堆上分配的,函数作用域外不会被销毁 ***
* 频繁调用场合并不适合new，就像new申请和释放内存一样
* 栈的大小远小于堆的大
* 栈是机器系统提供的数据结构，计算机会在底层对栈提供支持：分配专门的寄存器存放栈的地址，压栈出栈都有专门的指令执行,这就决定了栈的效率比较高。堆则是C/C++函数库提供的，它的机制是很复杂的，例如为了分配一块内存，库函数会按照一定的算法（具体的算法可以参考数据结构/操作系统在堆内存中搜索可用的足够大小的空间，如果没有足够大小的空间（可能是由于内存碎片太多）,就有可能调用系统功能去增加程序数据段的内存空间，这样就有机会分到足够大小的内存，然后进行返回。显然，堆的效率比栈要低得多.

    
------
假设有一个类 C，具有多个字段 X、Y、Z 等需要进行初始化，同理地，您可以使用上面的语法，只需要在不同的字段使用逗号进行分隔，如下所示：
```C++
C::C( double a, double b, double c): X(a), Y(b), Z(c)
{
  ....
}
```
    
例如: 
```C++
Line::Line( double len): length(len)
{
    cout << "Object is being created, length = " << len << endl;
}
```
    
上面的语法等同于如下语法:   
```C++
Line::Line( double len)
{
    cout << "Object is being created, length = " << len << endl;
    length = len;
}
```
    
    
类的析构函数与构造函数类似,但是构造函数在创建对象的时候,析构函数使用会在每次删除所创建的对象时执行,名称与类的名称是完全相同的,只是在前面加了个波浪号（~）作为前缀.析构函数有助于在跳出程序（比如关闭文件、释放内存等）前释放资源.
例如:
```C++
#include <iostream>
#include <string.h>
using namespace std;
class Test{
    public:
        char* getName(){
            return name;
        }
        void setName(char* name){
            strcpy(this->name, name);
        }
        Test(){}
        Test(int num);
        ~Test(){
            cout<<"this object is deleted"<<endl;
        }
        void sayHello(){
            int i;
            for(i=0;i<times;i++)
                cout<<"hello world"<<endl;
            cout<<endl;
        }
    private:
        char name[50];
        int times = 1;

};
Test::Test(int num): times(num){
    cout<<"this is a Hello object"<<endl;
}
int main(){
    Test test1;
    Test test2(3);
    test1.sayHello();
    test2.sayHello();
    return 0;
}
```
    
结果为:    
```
this is a Hello object
hello world

hello world
hello world
hello world

this object is deleted
this object is deleted
```
    
------
如果把函数成员声明为静态的，就可以把函数与类的任何特定对象独立开来。静态成员函数即使在类对象不存在的情况下也能被调用，静态函数只要使用类名加范围解析运算符 :: 就可以访问。
静态成员函数只能访问静态数据成员，不能访问其他静态成员函数和类外部的其他函数。
静态成员函数有一个类范围，他们不能访问类的* this *指针。您可以使用静态成员函数来判断类的某些对象是否已被创建。
```
#include <iostream>
 
using namespace std;

class Box
{
   public:
      static int objectCount;
      // 构造函数定义
      Box(double l=2.0, double b=2.0, double h=2.0)
      {
         cout <<"Constructor called." << endl;
         length = l;
         breadth = b;
         height = h;
         // 每次创建对象时增加 1
         objectCount++;
      }
      double Volume()
      {
         return length * breadth * height;
      }
      static int getCount()
      {
         return objectCount;
      }
   private:
      double length;     // 长度
      double breadth;    // 宽度
      double height;     // 高度
};

// 初始化类 Box 的静态成员
int Box::objectCount = 0;

int main(void)
{
  
   // 在创建对象之前输出对象的总数
   cout << "Inital Stage Count: " << Box::getCount() << endl;

   Box Box1(3.3, 1.2, 1.5);    // 声明 box1
   Box Box2(8.5, 6.0, 2.0);    // 声明 box2

   // 在创建对象之后输出对象的总数
   cout << "Final Stage Count: " << Box::getCount() << endl;

   return 0;
}
```
    
输出: 
```
Inital Stage Count: 0
Constructor called.
Constructor called.
Final Stage Count: 2
```
    
-----
C++引用   
引用就是某一变量（目标）的一个* 别名 *，* 对引用的操作与对变量直接操作完全一样 *。
引用的声明方法：类型标识符 &引用名=目标变量名;
` int a; int &ra=a; `//定义引用ra，它是变量a的引用，即别名
说明：
（1）&在此不是求地址运算，而是起标识作用。
（2）类型标识符是指目标变量的类型。
（3）声明引用时，必须同时对其进行初始化。
（4）<span style="color:red">引用声明完毕后，相当于目标变量名有两个名称，即该目标原名称和引用名，且不能再把该引用名作为其他变量名的别名.</span>
ra=1; 等价于 a=1;
（5）<span style="color:red">声明一个引用，不是新定义了一个变量，它只表示该引用名是目标变量名的一个别名，它本身不是一种数据类型，因此引用本身不占存储单元，系统也不给引用分配存储单元。故：对引用求地址，就是对目标变量求地址。&ra与&a相等。</span>
也可以这么的解释，所谓的引用（Reference），就是给对象取一个别名，使用该别名的时候可以取该对象。换句话说，是使新对象和原对象公用一个地址。这样，无论对哪个对象进行修改，其实都是对同一个地址的内容进行修改，因而原对象和新对象（规范的说，是对象和它的引用）总的来说具有相同的值。
    
A &a这个时候a是A的别名，相当于给类A又起了个名字，以后对a的操作都是对A的操作，一般情况下是用在函数形参的时候，在函数中操作类相当操作实参的类，和指针比起来，效率更快，因为是别名，不用分配新的内存.
    
-----
运算符重载   
重载的运算符是带有特殊名称的函数，函数名是由关键字 ***operator***和其后要重载的运算符符号构成的.与其他函数一样，重载运算符有一个返回类型和一个参数列表.     
例如: 
```C++
#include <iostream>
using namespace std;
class Box{
    public:
        int getVolume(){
            return height*length*width;
        }
        Box(int h, int w, int l){
            this->height = h;
            this->width = w;
            this->length = l;
        }
        int getWidth(){
            return width;
        }
        int getLength(){
            return length;
        }
        int getHeight(){
            return height;
        }
        Box operator+(const Box& a){
            Box box(this->getHeight()+a.getHeight(), this->getWidth()+a.getWidth(), this->getLength()+a.getLength());
            return box;
        }
    private:
        int height;
        int width;
        int length;
};
int main(){
    Box box1(1, 2, 3);
    Box box2(1, 2, 3);
    Box box3 = box1+box2;
    cout<<box1.getVolume()<<endl<<box2.getVolume()<<endl<<box3.getVolume()<<endl;
    return 0;
}
```
------  
C++中子类调用父类的构造器: 
```C++
class Flowerpot: public Box{
    public:
        Flowerpot(int h, int w, int l, char* plant): Box(h, w, l){
            strcpy(this->plant, plant);
        }
        char* getPlant(){
            return plant;
        }
    private:
        char plant[50];
};
```
----
const对象不能调用非const函数，因为非const函数不能保证不修改const对象的数据
```C++
#include <iostream>
using namespace std;
class Box{
private:
    int width;
public:
    Box(int width){
        this->width = width;
    }
    int getWidth() const;
    virtual int getVolume() = 0;
    friend void print(const Box&);
};
void print(const Box& box){
    cout<<box.getWidth();
}
int Box::getWidth() const{
    return width;
}
class Tangle: public Box{
public:
    Tangle(int width):Box(width){}
    int getVolume(){
//        return Box::getWidth()*Box::getWidth();
//        return getWidth()*getWidth();
        return Tangle::getWidth()*Tangle::getWidth();
    }
};
int main(){
    Box* box = new Tangle(3);
    print(*box);
    delete box;
    return 0;
}
```
----
####命名空间：
```C++
#include <iostream>
using namespace std;
namespace first_space{
    void func(){
        cout<<"this is the first space"<<endl;
    }
    namespace second_space{
        void func(){
            cout<<"this is the second space"<<endl;
        }
    }
}
//using namespace first_space;
//using namespace first_space::second_space;
int main(){
    first_space::func();
    first_space::second_space::func();
    return 0;
}
```
