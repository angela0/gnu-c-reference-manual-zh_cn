既然我们已经了解了 C 语言所有的基本元素，是时候宏观地看一下了。


### 6.1 程序结构 { #Program-Structure }

一个 C 程序可以只有一个单独的源文件，但通常会更多。一个比较大的程序会包含一些自定义的头文件和源文件，也会包含和链接已存在的库文件。

为了方便，头文件（带 “.h” 扩展）包含变量和函数的声明，源文件（带 “.c” 扩展）包含对应的定义。如果一些声明不想被其他文件看到，可以把该声明放到源文件中。然而几乎可以肯定的是，头文件不应该包含任何定义。

例如，如果你写了一个函数来计算平方根，并且你想让该函数可以被其他文件访问，而不仅仅是定义它的文件，你就可以会把该函数的声明放在头文件中（带 “.h” 扩展）：

```
/* sqrt.h */

double
computeSqrt (double x);
```

这个头文件可以被其他需要该函数的源文件包含，但并不需要知道这个函数如何实现的。

这个函数的实现在对应的源文件中（带有 “.c” 扩展）：

```
/* sqrt.c */
#include "sqrt.h"

double
computeSqrt (double x)
{
    double result;
    …
    return result;
}
```

### 6.2 作用域 { #Scope }

作用域指示了程序的哪一部分可以“看到”一个声明过的对象。一个声明过的对象可以仅在特定函数可见，可以在特定文件可以，也可以通过包含头文件并使用 `extern` 声明来对所有文件可见。

除非显式指出，否则在文件顶层的声明（就是不在一个函数里面）对整个文件可见，包括函数里面，但是对文件外不可见。

在函数里面的声明只对该函数内可见。

一个声明在该名字声明之前是不可见的，例如：

```
int x = 5;
int y = x + 10;
```
可以工作，但

```
int x = y + 10;
int y = 5;
```
不能工作。