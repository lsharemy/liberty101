title: vector的简单实现
tags:
  - C
id: 1942
categories:
  - wordpress
  - index.php
  - csit
date: 2012-10-11 17:22:19
---

实现一个vector，学习到了C++中的不少概念。

1.用explicit来防止隐式转换的发生
2.用形参的默认值来减少构造函数的个数
3.复制构造函数和赋值操作符的定义
4.vector的内存分配机制

[c]
template &lt;class T&gt;
class Vector
{
public:
    explicit Vector (int initSize = 0)
    : theSize(initSize), theCapacity(initSize)
    {
        objects = new T[theCapacity];
    }
    Vector(const Vector &amp; rhs):objects(NULL)
    {
        operator=(rhs);
    }
    const Vector &amp; operator=(const Vector &amp; rhs)
    {
        if (this != &amp;rhs)
        {
            delete [] objects;
            theSize = rhs.size();
            theCapacity = rhs.theCapacity;

            objects = new T[capacity()];
            for (int i = 0; i &lt; size(); i++)
                objects[i] = rhs.objects[i];
        }
        return *this;
    }
    ~Vector()
    {
        delete [] objects;
    }

    void resize(int newSize)
    {
        if (newSize &gt; theCapacity)
            reserve(newSize);
        theSize = newSize;
    }

    void reserve( int newCapacity)
    {
        T *oldArray = objects;
        objects = new T[newCapacity];
        for ( int i = 0; i &lt; theSize; i++)
            objects[i] = oldArray[i];
        theCapacity = newCapacity;
        delete [] oldArray;
    }

    int size() const
    {
        return theSize;
    }

    int capacity() const
    {
        return theCapacity;
    }

    void push_back(const T&amp; x)
    {
        if (theSize == 0 &amp;&amp; theCapacity == 0)
            reserve(1);
        else if (theSize == theCapacity)
            reserve(2*theCapacity);
        objects[theSize++] = x;
    }

    void pop_back()
    {
        theSize--;
    }

private:
    int theSize;
    int theCapacity;
    T *objects;
};
[/c]