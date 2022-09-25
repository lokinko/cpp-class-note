# C++ 程序设计与算法 笔记
课程地址: [《程序设计与算法（三）C++面向对象程序设计 北京大学 郭炜》](https://www.bilibili.com/video/BV1ob411q7vb?vd_source=1ac585a111fc39c9f7b30de84186453f)

## 0. 引用

### 0.1 变量的引用

某个变量的引用，等价于该变量。定义引用时一定要初始化为引用某个变量而非常量；

在函数传入形参时，可以通过引用的方式在函数里操作传入值，不然形参的改变无法直接作用到实参中；

```
int n = 4;
int &r = n;  // r 引用了 n，r 的类型是 int &；
```

r 可以看成 n 的别名， & r 为 n 的地址；



### 0.2 函数返回值的引用

 此外，可以通过对函数返回值定义为引用的方式使用；

```
int n = 4;
int &SetValue()
{
	return n;
}
```



### 0.3 常引用

常引用定义后不能通过引用去修改被引用的内容，通过 const 关键字定义常引用；

```
int n = 100;
const int &r = n;
```

**Const T & 和 T & 两种引用方式是不同的类型，无法相互转换！**



## 1. Const 关键字

### 1.1 定义常量

通过 const 关键字定义一些不能被改动的数据，防止被错误赋值；

```
const int MAX_VAL = 23;
const double Pi = 3.14;
const char * SCHOOL_NAME = "Perking University";
```



### 1.2 定义常量指针

不可以通过常量指针修改其指向的内容；可以将常量指针赋值给非常量指针，但不能反过来，因为 const 关键字倾向于保护被指向的内容不被修改。

```
int n, m;
const int *p = & n;
*p = 5;   // 非法操作，不可以通过常量指针改变指向的对象；
n = 4;    // 可以直接改变对象的值；
p = &m;   // 常量指针的指向也可以变化
```

函数参数为 **「常量指针」**时，可以避免函数内部不小心改变参数指针所指地方的内容。

```
Void MyPrint(const char *p)
{
	strcpy(p, "this");  // 编译出错
	printf("%s", p);
}
```



## 2. 动态内存分配

### 2.1 new 运算符分配一个变量

动态分配出一块大小为 sizeof(T) 字节的内存空间，并将该空间的**「起始地址」**赋值给 P；

```
int * pn;
pn = new int;
*pn = 5;
```



### 2.2 new 运算符分配一个数组

动态分配出一块大小为 n*sizeof(T) 字节的内存空间，并将该空间的**「起始地址」**赋值给 P；

```
int *pn;
pn = new int[100];
pn[0] = 20;
```



### 2.3 delete 运算符释放动态分配的内存

用**「new」**动态分配的内存空间一定要用**「delete」**运算符进行释放；

```
// 释放变量的内存
int *p = new int;
*p = 5;
delete p;

// 释放数组的内存
int *p = new int[100];
p[0] = 100;
delete [] p;
```



## 3. 内联函数及函数重载

### 3.1 内联函数

内联函数是用来解决对于函数比较简单的情况下直接调用，用整个函数代码插入替换原有位置的函数名；

```
inline int Max(int a, int b)
{
	if(a > b) return a;
	return b;
}
```



### 3.2 函数重载

一个或多个函数名相同，但传入的参数个数或参数类型不同，就会执行不同的函数操作；

函数重载能让函数命名更简单，编译器根据参数个数以及类型来选择调用相应的函数执行；

```
int Max(double f1, double f2){};
int Max(int n1, int n2){};
int Max(int n1, int n2, int n3){};
```

函数参数可以有缺省值，函数重载中需要避免因缺省值导致的冲突；



## 4. 面向对象的 C++ 程序设计

### 4.1 类的构造

类是一种带有数据操作的数据类型，将数据结构和操作该数据结构的函数捆绑形成类，通过对数据的操作进行抽象与封装；

```
class CRectangle
{
public:
    int w, h;
    int Area();
    int Perimeter();
    void Init(int w_, int h_);
};

int CRectangle::Area(){
    return w*h;}
int CRectangle::Perimeter(){
    return 2 * (w + h);}
int CRectangle::Init(int w_, int h_){
    w = w_; h = h_;}
```

和结构变量一样，对象所占用的内存空间大小等于所有成员变量的大小之和；

此外，类成员的访问范围为：public，private，protected；

在类成员函数内部可访问：

	- 当前对象的全部属性、函数；
	- 同类其他对象的全部属性、函数；

在类成员函数外的地方，只能访问该类对象的公有成员；

```
class CEmployee{
	private:
		char szName[30];
	public:
		int salary;
		void setName(char * name);
		void getName(char * name);
}

void CEmployee::setName(char * name){
	strcpy(szName, name);
}

void CEmployee::getName(char *name){
	strcpy(name, szName);
}
```



### 4.2 构造函数

如果定义类的时候没有定义构造函数，则编译器会生成默认构造函数；一个类可以有多个构造函数；

```
// 类的定义
class Complex{
	private:
		double real, imag;
	public:
		Complex(double r, double i = 0);
		void getComplex();
}

// 构造函数的定义
Complex::Complex(double r, double i){
	real = r; imag = i;
}

void Complex::getComplex(){
    cout << "real is: " << real << ", " << "imag is: " << imag << endl;
}

// 对象的实例化
Complex c0(3, 5);
Complex c1[2] = {1, 3};
Complex c2[2] = {Complex(3,4), Complex(5,6)};
```



### 4.3 复制构造函数

只有一个参数，即对同类对象的引用；如果没有定义复制构造函数，编译器会自动生成。形如：

```
X::X(X&) 或 X::X(const X&)
```

三种触发复制构造函数的例子：

	1. 用一个对象去初始化另一个同类对象；
	1. 该类被作为形参传入时，在函数调用时会同时调用复制构造函数；
	1. 函数的返回值时该类的对象，则函数返回时会同时调用复制构造函数；

**仅使用赋值语句时并不会触发复制构造函数，例如：**

```
complex c1, c2;
c1.real = 5;
c2 = c1;    // 此时仅实现赋值；
```

如果想避免调用复制构造函数的开销，可以通过引用形式进行调用，用 const 关键词保护参数：

```
void fun(const CMyclass & obj){
	cout << "fun" << endl;    // 函数中任何试图改变 obj 的语句都将报错；
}
```



### 4.4 析构函数

析构函数在对象生命周期结束时执行，名字与类名相同，在前面加上~作为标识;

析构函数没有参数与返回值，一个类最多只能有一个析构函数。

```
class String{
	private:
		char *p;
	public:
		String();
		~String();
}

String::String(){
	p = new char[10];
}

String::~String(){
	delete [] p;
}
```

对象生命周期结束的几种常见情况：

	1. new 产生的对象在 delete 后消亡；
	1. 作为形参的对象消亡时也会导致析构函数调用；
	1. 函数返回对象后会导致析构函数调用；



## 5. this 指针

应用于 C++ 翻译到 C 程序的指针变量，用于指向 C++ 的成员函数作用的对象；

```
// C++ 程序
Class CCar{
	public:
		int price;
		void SetPrice(int p);
};

void CCar::SetPrice(int p){
	price = p;
}

int main(){
	CCar car;
	car.SetPrice(20000);
	return 0
}
```

翻译为 C 程序时，需要通过 this 指针指向成员函数作用的对象：

```
struct CCar{
	int price;
};

void SetPrice(struct CCar * this, int p){
	this->price = p;
}

int main(){
	struct CCar car;
	SetPrice(& car, 20000);
	return 0;
}
```

静态成员函数中不能使用 this 指针，因为静态成员函数并不具体作用与某个对象！

静态成员函数的真实参数的个数，就是程序中写出来的参数；


## 6. 静态成员

静态成员指在说明前面加了 static 关键字的成员；静态成员变量为所有对象共享；sizeof运算符不会计算静态成员变量；

```
class CRectangle{
	private:
		int w, h;
		static int nTotalArea;
		static int nTotalNumber;
	public:
		CRectangle(int w_, int h_);
		~CRectangle();
		static void PrintTotal();
};
```

普通成员变量必须具体作用于某个对象，而静态成员函数并不具体作用于某个对象；

因此，静态成员不需要通过对象就能访问；

```
1. 	CRectangle::PrintTotal();
2. 	CRectangle r;
	r.PrintTotal();
3. 	CRectangle *p = &r;
	p->PrintTotal();
4. 	CRectangle & ref = r;
	int n = ref.nTotalNumber;
```

** **必须在定义类的文件中对静态成员变量进行说明或初始化，否则编译能通过，但链接不能通过 ****

** **静态成员函数中不能访问非静态成员变量，也不能调用非静态成员函数 ****



## 7. 成员对象和封闭类：

**有成员对象的类叫封闭（enclosing）类**

```
class CTyre
{
	private:
		int radius;
		int width;
	public:
		CTyre(int r, int w):radius(r), width(w){}
};

class CEngine
{
};

class CCar
{
	private:
		int price;
		CTyre tyre;
		CEngine engine;
	public:
		CCar(int p, int tr, int tw);
}

CCar::CCar(int p, int tr, int tw):price(p), tyre(tr, w)
{
};

int main()
{
	CCar car(20000, 16, 225);
	return 0;
}
```

任何生成封闭类对象的语句，都要让编译器明白，对象中的成员对象如何初始化；

通过封闭类的构造函数的初始化列表；



## 8. 常量对象

如果不希望某个对象的值被改变，则定义该对象的时候可以加上 const 关键字;

** **常量成员函数在执行期间不应修改其作用的对象，也不能调用同类的非常量成员函数** **

两个成员函数参数与函数名都相同，但是其中一个是 const，算函数重载；

常量对象只能调用常量成员函数，但非常量对象也可以调用常量成员函数；

```
class Sample
{
	public:
		int value;
		void GetValue() const;
		void func(){};
		Sample(){};
};

void Sample::GetValue() const
{
	value = 0;
	func();
}

int main()
{
	const Sample o;
	o.value = 100;    // error, 常量对象不可被修改;
	o.func();		 // error, 常量对象上不能执行非常量成员函数;
	o.GetValue();	 // ok, 常量对象上可以执行常量成员函数;
	return 0;
}
```

## 9. 运算符重载
运算符重载的实质是函数重载，其目的是用于扩充自定义类型数据的相关操作，例如不同自定义类型相加减等；
```
返回值类型 operator 运算符(形参表)
{
    ......
}
```
重载函数为成员函数时，参数个数为运算符目数减一；
重载函数为普通函数时，参数个数为运算符目数；
```
class Complex
{
    public:
	    double real, imag;
		Complex(double r=0.0, double i=0.0): real(r), imag(i) {    }
		Complex operator-(const Complex & c);
};

// 全局函数，非 Complex 对象的成员函数
Complex operator+(const Complex & a, const Complex & b)
{
	return Complex(a.real + b.real, a.imag + b.imag);
}

// Complex 对象的成员函数
Complex Complex::operator-(const Complex & c)
{
	return Complex(real - c.real, imag - c.imag);
}
```
Note: 经测试发现对于同一种运算操作，全局函数好像会覆盖类成员函数，例：
```
Complex Complex::operator+(const Complex & c)
{
	return Complex(real + 2*c.operator_real, imag + 2*c.operator_imag);
}

Complex operator+(const Complex & a, const Complex & b)
{
	return Complex(a.operator_real + b.operator_real, a.operator_imag + b.operator_imag);
}

int main()
{
    Complex a(3,3), b(2,2);
    Complex c = a.operator+(b);
    cout << c.operator_real << "\n" << c.operator_imag << endl;
    Complex d = a + b;
    cout << d.operator_real << "\n" << d.operator_imag << endl;
    return 0;
}

# 结果均为 (7, 7)
```

### 9.2 赋值运算符重载
目的：希望赋值运算符两边的类型可以不匹配，通过赋值运算符将其赋值给对象；** **赋值运算符只能重载为成员函数** **
```
class String {
    private:
        char * str;
    public:
        String ():str(new char[1]) { str[0] = 0;}
        const char * c_str() { return str;};
        String & operator=(const char *s);
        ~String();
};

String & String::operator=(const char * s)
{
    delete [] str;
    str = new char[strlen(s) + 1];
    strcpy(str, s);
    return * this;
}

String::~String() {
    delete [] str;
}
```
其中，运算符重载可以看成是一个新的函数```class.operator=()```,可以等同于一般的函数进行操作
```
int main() {
	String s;
	s = "Good luck,";        // 等价于 s.operator=("Good luck,");
	String s2 = "hello!";    // 函数报错：这里的“=”不是赋值语句，而是初始化语句
	return 0;
}
```

### 9.3 浅拷贝和深拷贝
```
String S1, S2;
S1 = "this";
S2 = "that";
S1 = S2;
```
如果不定义赋值运算符，会导致```S1 = S2```使得 ```S1.str```和```S2.str```指向同一地方；
- ```S1.str```所指向的地址无法访问，成为内存垃圾；
- ```S1.str```执行析构函数释放空间后，```S2.str```还会释放一次空间，造成错误；
- 针对```S1 = "other"```后，会导致 ```S2.str```指向的地方被 ```delete```;

因此需要对运算符重载，为```class.operator=```添加重载操作；
```
String & String::operator=(const String & s)
{
	if(this == & s) {
		return * this;	// 判断当前引用是否为对象本身；
	}
    delete [] str;
    str = new char[strlen(s) + 1];		// C语言中 char* 最后以 "\0" 结尾；
    strcpy(str, s);
    return * this;
}
```
```"class.operator="```返回值类型：尽量保持运算符原本的特性；

 ```
a = b = c
等价于：a.operator=(b.operator=(c));
(a = b) = c
等价于：(a.operator=(b)).operator=(c);
```

### 9.4 运算符重载为友元函数
一般情况下，将运算符重载为类的成员；
但有时候重载为成员函数不能满足使用需求，重载为普通函数又不能访问类的私有成员，所以需要重载运算符为友元；
```
// 全局函数写法
Complex operator+(double r, const Complex & c) {
	return Complex(c.real + r, c.imag);
}

//类成员函数写法
Complex Complex::operator+(double r) {
	return Complex(real+r, imag);
}

这样就能实现:
c = c + 5
c = 5 + c
两个操作同时满足全局函数写法的实现；
```

## 10. 可变长数组类的实现
很多场景下需要对class中数据存放的长度实现可变输入，包括添加元素，赋值，指定某index的值等，通过运算符重载和复制构造函数等方式可以实现这些功能；
**可以通过模板类```<template>```的方式实现数据类型的扩展；**
```
class CArray {
public:
	// 构造函数
	CArray(int s=0);
	// 复制构造函数
	CArray(CArray &a);
	// 析构函数
	~CArray();
	int length() { return size;};
	// 返回引用才能在赋值过程 a[i] = 4 生效
	int& operator[](int i) { return ptr[i];};
	void operator=(const CArray &a);
	void push_back(int v);
private:
	int size;
	int *ptr;
}

CArray::CArray(int s):size(s) {
	if(s == 0) {
		ptr = NULL;
	}
	else{
		ptr = new int[s];
	}
}

CArray::CArray(CArray &a) {
	if(!a.ptr) {
		ptr = NULL;
		size = 0;
		return;
	}
	ptr = new int[a.size];
	memcpy(ptr, a.ptr, sizeof(int)*a.size);
	size = a.size;
}

CArray::~CArray() {
	if(ptr) {delete [] ptr;}
}

CArray & CArray::operator=(const CArray &a) {
	if(ptr == a.ptr) {
		return *this;
	}
	if(a.ptr == NULL) {
		if(ptr) {
			delete [] ptr;
		}
		ptr = NULL;
		size = 0;
		return *this;
	}
	if(size < a.size) {
		if(ptr) {
			delete [] ptr;
		}
		ptr = new int[a.size];
	}
	memcpy(ptr, a.ptr, sizeof(int)*a.size);
	size = a.size;
	return *this
}

void CArray::push_back(int v) {
	if(ptr) {
		int * tmpPtr = new int[size+1];
		memcpy(tmpPtr, ptr, sizeof(int)*size);
		delete [] ptr;
		ptr = tmpPtr;
	}
	else {
		ptr = new int[1];
	ptr[size++] = v;
	}
}
```