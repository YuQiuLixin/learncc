**C++对象模型简述**

-----

> 在C++中,有两种类的成员变量:static和非static,有三种成员函数:static 非static和virtual.

- 含有非static成员变量及成员函数的类的对象的内存分布 

```c++
class Person
{
    public:
        Person():mId(0), mAge(20){}
        void print()
        {
            cout << "id: " << mId
                 << ", age: " << mAge << endl;
        }
    private:
        int mId;
        int mAge;
}; 

int main()
{
    Person p1;
    cout << "sizeof(p1) == " << sizeof(p1) << endl;
    int *p = (int*)&p1;
    cout << "p.id == " << *p << ", address: "  << p << endl;
    ++p;
    cout << "p.age == " << *p << ", address: " << p << endl;
    cout << endl;
    
    Person p2;
    cout << "sizeof(p2) == " << sizeof(p2) << endl;
    p = (int*)&p2;
    cout << "p.id == " << *p << ", address: " << p << endl;
    ++p;
    cout << "p.age == " << *p << ", address: " << p << endl;
    return 0;
} 

运行结果
sizeof(p1) == 8
p1.id == 0, address: 0x7fff5ca55b40
p1.age == 20, address: 0x7fff5ca55b44

sizeof(p2) == 8
p2.id == 0, address: 0x7fff5ca55b30
p2.age == 20, address: 0x7fff5ca55b34
```

使用普通的int＊指针可以遍历输出对象内的非static成员变量的值，且两个对象中的相同的非static成员变量的地址各不相同。 

据此，可以得出结论，在C++中，非static成员变量被放置于每一个类对象中，非static成员函数放在类的对象之外，且非static成员变量在内存中的存放顺序与其在类内的声明顺序一致。即person对象的内存分布如下图所示：

![1](/Users/tatamaaki/Desktop/1.png)

- 含有static和非static成员变量和成员函数的类的对象的内存分布

加static成员，内存结构不变，static变量放到类的对象之外

- 加入virtual成员函数的类的对象的内存分布

```c++
#include <iostream>
using namespace std;

class Person
{
    public:
        Person():mId(0), mAge(20){}
        virtual void func1()  
        {  
            cout << "Person--func1" << endl;
        }  
        virtual void job()  
        {  
            cout << "Person--job" << endl;  
        }  
        virtual ~Person()  
        {  
            cout << "~Person" << endl;  
        }  
    private:
        int mId;
        int mAge;
}; 


//visit the virtual function, the sysytem is X64, use the long type
void visitVtbl(long **vtbl, int count)  
{  
    cout << vtbl << endl;  
    //cout << "\t[-1]: " << (long)vtbl[-1] << endl;  
  
    typedef void (*FuncPtr)();  
    for (int i = 0; vtbl[i] && i < count; ++i)  
    {  
        cout << "\t[" << i << "]: " << vtbl[i] << " -> ";  
        FuncPtr func = (FuncPtr)vtbl[i];  
        func();  
    }  
}  

int main()  
{  
    Person person;  

    visitVtbl((long **)(*((long *)&person)),3)
	 
    return 0;  
} 

输出结果：
输出虚函数的地址以及运行虚函数结果
```

visitvtbl提供count参数，因为并不是所有的虚函数表都以0结尾，同时实验系统为64位指针，此处使用long *， 传递一个二级指针，内存结构如下图（添加了vfptr 虚函数表指针）

![20150522023548880](/Users/tatamaaki/Desktop/20150522023548880.png)

