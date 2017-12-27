---
layout: default
title: PImpl Idiom in C++11
---


# PImpl Idiom in C++11

### Pointer to Implementation

PImpl idiom, 一种C++实现接口和实现分离的方法。它的优点如下：

- 加快编译速度
- 隐藏实现，不暴露给使用者

实现方法是，在类的头文件中声明一个新类型的指针，在类的源文件中去定义新类型包含的成员。通过这种方式，即使新类型成员发生变化，其他依赖于该类的源文件无需重新编译，因为类头文件中声明的是指针，指针本身没有发生变化，指向的内容有变化。

### Implementation in C++98
一个没有使用PImpl的例子，定义一个图书类

```
/* book.h */
class Book
{
public:
  void print();
private:
  std::string  m_Contents;
}
```
当书的成员发生变化时，比如增加title，class Book就发生了变化。

```
/* book.h */
class Book
{
public:
  void print();
private:
  std::string  m_Contents;
  std::string  m_Titles;
}
```
依赖book.h的源文件都要重新编译。
如果使用PImpl的方式

```
/* book.h */
class Book
{
public:
  Book();
  ~Book();
  void print();
private:
  class BookImpl;
  BookImpl* const m_p;
}
```

```
/* book.cpp */
#include "public.h"
#include <iostream>
class Book::BookImpl
{
public:
  void print();
private:
  std::string  m_Contents;
  std::string  m_Title;
}
Book::Book(): m_p(new BookImpl())
{
}
Book::~Book()
{
  delete m_p;
}
void Book::print()
{
  m_p->print();
}
/* then BookImpl functions */
void Book::BookImpl::print()
{
  std::cout << "print from BookImpl" << std::endl;
}
```
PImpl就完成了。

### Implementation in C++11
C++11引入了智能指针，用RAII的方式去管理资源。使用unique_ptr实现的版本

```
/* book.h */
class Book
{
public:
  Book();
  ~Book();
  void print();
private:
  class BookImpl;
  std::unique_ptr<BookImpl> m_p;
}
```

```
/* book.cpp */
#include "public.h"
#include <iostream>
class Book::BookImpl
{
public:
  void print();
private:
  std::string  m_Contents;
  std::string  m_Title;
}
Book::Book(): m_p(std::make_unique<Impl>())
{
}
/* Should define the deconstruct function otherwise compiling error */
Book::~Book() = default;
void Book::print()
{
  m_p->print();
}
/* then BookImpl functions */
void Book::BookImpl::print()
{
  std::cout << "print from BookImpl" << std::endl;
}
```

shared_ptr实现版本

```
/* book.h */
class Book
{
public:
  Book();
  void print();
private:
  class BookImpl;
  std::shared_ptr<BookImpl> m_p;
}
```

```
/* book.cpp */
#include "public.h"
#include <iostream>
class Book::BookImpl
{
public:
  void print();
private:
  std::string  m_Contents;
  std::string  m_Title;
}
Book::Book(): m_p(std::make_shared<Impl>())
{
}
void Book::print()
{
  m_p->print();
}
/* then BookImpl functions */
void Book::BookImpl::print()
{
  std::cout << "print from BookImpl" << std::endl;
}
```
