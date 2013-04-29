﻿
#  C++ Template 进阶指南

## 0. 前言

###0.1 C++另类简介：比你用的复杂，但比你想的简单

C++似乎从他为世人所知的那天开始便成为天然的话题性编程语言。在它在周围有着形形色色的赞美与贬低之词。当我在微博上透露欲写此文的意愿时，也收到了很多褒贬不一的评论。作为一门语言，能拥有这么多使用并恨着它、使用并畏惧它的用户，也算是语言丛林里的奇观了。

C++之所以变成一门层次丰富、结构多变、语法繁冗的语言，是有着多层次的原因的。Bjarne在《The Design and Evolution of C++》一书中，详细的解释了C++为什么会变成如今（C++98/03）的模样。这本书也是我和陈梓瀚一直对各位已经入门的新手强烈推荐的一本书。通过它你多少可以明白，C++的诸多语法要素之所以变成如今的模样，实属迫不得已。

模板作为C++中最有特色的语言特性，它堪称玄学的语法和语义，理所应当的成为初学者的梦魇。甚至很多工作多年的人也对C++的模板部分保有充分的敬畏。在多数的编码标准中，Template俨然和多重继承一样，成为了一般程序员（非程序库撰写者）的禁区。甚至运用模板较多的Boost的，也成为了“众矢之的”。

但是实际上C++模板远没有想象的那么复杂。我们只需要换一个视角：在C++03的时候，模板本身就可以独立成为一门“语言”。它有“值”，有“函数”，有“表达式”和“语句”。除了语法比较蹩脚外，它既没有指针也没有数组，更没有C++里面复杂的继承和多态。可以说，它要比C语言要简单的多。如果我们把模板当做是一门语言来学习，那只需要花费学习OO零头的时间即可掌握。按照这样的思路，可以说在各种模板书籍中出现的多数技巧，都可以被轻松理解。

简单回顾一下模板的历史。87年的时候，泛型（Generic Programming）变被纳入了C++的考虑范畴，并直接导致了后来模板语法的产生。可以说模板语法一开始就是为了在C++中提供泛型机制。92年的时候，Alexandar Stepanov开始研究利用模板语法制作程序库，后来这一程序库发展成STL，并在93年被接纳入标准中。

此时不少人以为STL已经是C++模板的集大成之作，C++模板技止于此。但是在95年的《C++ Report》上，John Barton和Lee Nackman提出了一个矩阵乘法的模板示例。可以说元编程在那个时候开始被很多人所关注。自此篇文章发表之后，很多人大牛都开始对模板产生了浓厚的兴趣。其中对元编程技法贡献最大的当属Alexandrescu的《Modern C++ Design》及模板程序库Loki。这一2001年发表的图书间接地导致了模板元编程库的出现。书中所使用的Typelist等泛型组件，和Policy等设计方法令人耳目一新。但是因为全书用的是近乎Geek的手法来构造一切设施，因此使得此书阅读起来略有难度。

2002年出版的另一本书《C++ Templates》，可以说是在Template方面的集大成制作。它详细阐述了模板的语法、提供了和模板有关的语言细节信息，举了很多有代表性例子。但是对于模板新手来说，这本书细节如此丰富，让他们随随便便就打了退堂鼓缴械投降。

本文的写作初衷，就是通过“编程语言”的视角，介绍一个简单、清晰的“模板语言”。我会尽可能的将模板的诸多要素连串起来，用一些简单的例子帮助读者学习这门“语言”，让读者在编写、阅读模板代码的时候，能像 `if(exp) { dosomething(); }`一样的信手拈来，让“模板元编程”技术成为读者牢固掌握、可举一反三的有用技能。

###0.2 适宜读者群

因为本文并不是用于C++入门，例子中也多少会牵涉一些其它知识，因此如果读者能够具备以下条件，会读起来更加轻松：

* 熟悉C++的基本语法；
* 使用过STL；
* 熟悉一些常用的算法，以及递归等程序设计方法。

此外，尽管第一章会介绍一些Template的基本语法，但是还是会略显单薄。因此也希望读者能对C++ Template最基本语法形式有所了解和掌握；如果会编写基本的模板函数和模板类那就更好了。

诚如上节所述，本文并不是《C++ Templates》的简单重复，与《Modern C++ Design》交叠更少。从知识结构上，我建议大家可以先读本文，再阅读《C++ Templates》获取更丰富的语法与实现细节，以更进一步；《Modern C++ Design》除了元编程之外，还有很多的泛型编程示例，原则上泛型编程的部分与我所述的内容交叉不大，读者在读完1-3章了解模板的基本规则之后便可阅读《MCD》的相应章节；元编程部分（如Typelist）建议在阅读完本文之后再行阅读，或许会更易理解。

###0.3 版权

本文是随写随即同步到Github上，因此在行文中难免会遗漏引用。本文绝大部分内容应是直接承出我笔，但是也不定会有他山之石。所有指涉内容我会尽量以引号框记，或在上下文和边角注记中标示，如有遗漏烦请不吝指出。

全文所有为我所撰写的部分，作者均保留所有版权。如果有需要转帖或引用，还请注明出处并告知于我。

## 1. Template的基本语法

###1.1 Template Class基本语法

####1.1.1 Template Class的与成员变量定义
我们来回顾一下最基本的Template Class声明和定义形式：

Template Class声明：
```C++
template <typename T> class ClassA;
```

Template Class定义：
```C++
template <typename T> class ClassA
{
	T member;
};
```

`template` 是C++关键字，意味着我们接下来将定义一个模板。和函数一样，模板也有一系列参数。这些参数都被囊括在template之后的`< >`中。在上文的例子中， `typename T`便是模板参数。回顾一下与之相似的函数参数的声明形式：

``` C++
void foo(int a);
```

`T`则可以类比为函数形参`a`，这里的“模板形参”`T`，也同函数形参一样取成任何你想要的名字；`typename`则类似于例子中函数参数类型`int`，它表示模板参数中的`T`将匹配一个类型。

在定义完模板参数之后，便可以定义你所需要的类。不过在定义类的时候，除了一般类可以使用的类型外，你还可以使用在模板参数中使用的类型 `T`。可以说，这个 `T`是模板的精髓，因为你可以通过指定模板实参，将T替换成你所需要的类型。

例如我们用`ClassA<int>`来实例化模板类ClassA，那么`ClassA<int>`可以等同于以下的定义：

``` C++
// 注意：这并不是有效的C++语法，只是为了说明模板的作用
typedef class {
	int member;
} ClassA<int>;
```

可以看出，通过模板参数替换类型，可以获得很多形式相同的新类型，有效减少了代码量。这种用法，我们称之为“泛型”（Generic Programming），它最常见的应用，即是STL中的容器模板类。

####1.1.2 模板的使用

对于C++来说，类型最重要的作用之一就是用它去产生一个变量。例如我们定义了一个动态数组（列表）的模板类`vector`，它对于任意的元素类型都具有push_back和clear的操作，我们便可以如下定义这个类：

```C++
template <typename T>
class vector
{
public:
	void push_back(T const&);
	void clear();				
	
private:
	T* elements;
};
```

此时我们的程序需要一个整型和一个浮点型的列表，那么便可以通过以下代码获得两个变量：

```C++
vector<int> intArray;
vector<float> floatArray;
```

此时我们就可以执行以下的操作，获得我们想要的结果：

```C++
intArray.push_back(5);
floatArray.push_back(3.0f);
```

变量定义的过程可以分成两步来看：第一步，`vector<int>`将`int`绑定到模板类`vector`上，获得了一个“普通的类`vector<int>`”；第二步通过“vector<int>”定义了一个变量。
与“普通的类”不同，模板类是不能直接用来定义变量的。例如

```C++
vector unknownVector; // 错误示例
```

这样就是错误的。我们把通过类型绑定将模板类变成“普通的类”的过程，称之为模板实例化（Template Instantiate）。实例化的语法是：
 
```
模板名 < 模板实参1 [，模板实参2，...] >
```

看几个例子：
```C++
vector<int>
ClassA<double>

template <typename T0, typename T1> class ClassB
{
	// Class body ...
};

ClassB<int, float>
```

当然，在实例化过程中，被绑定到模板参数上的类型（即模板实参）需要与模板形参正确匹配。
就如同函数一样，如果没有提供足够并匹配的参数，模板便不能正确的实例化。
 
####1.1.3 模板类的成员函数定义

由于C++11正式废弃“模板导出”这一特性，因此在模板类的变量在调用成员函数的时候，需要看到完整的成员函数定义。因此现在的模板类中的成员函数，通常都是以内联的方式实现。
例如：

``` C++
template <typename T>
class vector
{
public:
	void clear()
	{
		// Function body
	}
	
private:
	T* elements;
};
```

当然，我们也可以将`vector<T>::clear`的定义部分放在类型之外，只不过这个时候的语法就显得蹩脚许多：

```C++
template <typename T>
class vector
{
public:
	void clear();			// 注意这里只有声明
private:
	T* elements;
};

template <typename T>
void vector<T>::clear()		// 函数的实现放在这里
{
	// Function body
}
```

函数的实现部分看起来略微拗口。我第一次学到的时候，觉得

``` C++
void vector::clear()
{
	// Function body
}
```

这样不就行了吗？但是简单想就会知道，`clear`里面是找不到泛型类型`T`的符号的。

因此，在成员函数实现的时候，必须要提供模板参数。此外，为什么类型名不是`vector`而是`vector<T>`呢？
如果你了解过模板的偏特化与特化的语法，应该能看出，这里的vector<T>在语法上类似于特化/偏特化。实际上，这里的函数定义也确实是成员函数的偏特化。特化和偏特化的概念，本文会在第二部分详细介绍。

最终，正确的成员函数实现如下所示：

``` C++
template <typename T>		// 模板参数
void vector<T> /*看起来像偏特化*/ ::clear()		// 函数的实现放在这里
{
	// Function body
}
```

###1.2 Template Function的基本语法

####1.2.1 Template Function的声明和定义

模板函数的语法与模板类基本相同，也是以关键字`template`和模板参数列表作为声明与定义的开始。模板参数列表中的类型，可以出现在参数、返回值以及函数体中。比方说下面几个例子

```C++
template <typename T> void foo(T const& v);

template <typename T> T foo();

template <typename T, typename U> U foo(T const&);

template <typename T> void foo()
{
	T var;
	// ...
}
```

####1.2.2 模板函数的使用


###1.3 整型也可是Template参数

## 2.  模板世界的If-Then-Else：特化与偏特化
###2.1 类模板的匹配规则：特化与部分特化
###2.2 函数模板的重载、特化与部分特化
###2.3 技巧单元：模板与继承

## 3   拿起特化的武器，去写程序吧！
###3.1 利用模板特化规则实现If-Then-Else与Switch-Case
###3.2 特化可以有多个选择：替换失败并不是一个错误，只是一种可能
###3.3 技巧单元：获得类型的属性——类型萃取（Type Traits） 

## 4   用模板写程序吧！骚年！
###4.1 模板上的递归
###4.2 将循环变成递归，将分支变成递归，将一切变成递归
###4.3 实战单元：元编程的Fibonacci数列
###4.4 技巧单元：typename与template的另一种用法
###4.5 实战单元：撰写你自己的元编程“函数”库
###4.6 实战单元：实现元编程上的数据结构——以Vector为例

## 5   关于模板，你还需要知道的其它常识
###5.1 类中类：灵活的模板定义
###5.2 Template-Template Class
###5.3 技巧单元：高阶函数——从函数到函数的组合
###5.4 实战单元：STL中的Allocator Rebinder
###5.5 像看堆栈一样的看出错信息
###5.6 模板的症结：易于实现，难于完美

alexandrescu 关于 min max 的讨论：《再谈Min和Max》

## 6   C++11的新特性
###6.1 变参模板
###6.2 Lambda与模板程序

## 7   结语：讨论有益，争端无用