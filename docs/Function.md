你可以使用函数来将程序的一部分分离出来，作为单独的子过程。要写一个函数，至少必须要定义它。显式函数声明是个好习惯；你可以不声明，但默认的隐式声明可能匹配不到函数定义。这样你收到编译时警告。

每一个程序都至少需要一个叫做 `main` 的函数。这是程序的入口。

### 5.1 函数声明 { #Function-Declarations }

函数声明指定了函数的名字、参数列表和返回值类型。一个函数声明要以分号结束。下面是其一般形式：

```
return-type function-name (parameter-list);
```
*return-type* 指示了该函数返回的值的类型。你可以使用 `void` 作为返回值类型声明一个不返回任何值的函数。

*function-name* 可以是任何合法的标识符。（详见 [标识符](Lexical-Elements.md#Identifiers)）

*parameter-list* 包含 0 个或多个参数，用逗号隔开。一个参数通常包含数据类型和一个可选的名字。你可以声明一个拥有可变参数个数的函数（详见 [可变参数列表](#Variable-Length-Parameter-Lists)），或者使用 `void` 声明一个没有参数的函数。不写 `parameter-list` 的话也表示没有参数，但最好使用 `void` 指明。

下面的例子声明了一个带有 2 个参数的函数：

```
int foo (int, double);
```
如果要给一个参数加上名字，那名字要紧跟数据类型：

```
int foo (int x, double y);
```
参数名字可以是任意标识符（详见 [标识符](Lexical-Elements.md#Identifiers)），并且如果有多个参数你不能在一个声明里使用相同的名字。这里的参数名字不需要和定义中的名字匹配。

你要在第一次使用一个函数之前声明它。你可以把它放在头文件中，然后在使用该函数的源文件中用 `#include` 指令来包含该函数声明。

### 5.2 函数定义 { #Function-Definitions }

函数定义指明该函数实际都干什么。函数定义包含的信息有：函数名，返回类型，参数的类型和名字，以及函数体。函数体是一系列由花括号包起来的语句；实际上它就是一个代码块（详见 [代码块](Statement.md#Blocks)）。

其一般形式为：

```
return-type
function-name (parameter-list)
{
    function-body
}
```
*return-type* 和 *function-name* 和 函数声明中的是一样的（详见 [函数声明](#Function-Declarations)）。

*parameter-list* 基本和函数声明中的一样（详见 [函数声明](#Function-Declarations)），除了在函数定义时你必须要指定参数名字。

下面定义了一个函数 -- 它需要 2 个整型参数，并把它们的和作为返回值：

```
int
add_values (int x, int y)
{
    return x + y;
}
```
为了兼容 C 语言的初始设计，你可以在参数列表的右括号后指定参数类型。如下：

```
int
add_values (x, y)
    int x, int y;
{
    return x + y;
}
```
然而，我们强烈不推荐这种代码风格；这样写会造成细微的类型转换问题，及其他问题。

### 5.3 调用函数 { #Calling-Functions }

你可以使用函数名字来调用函数，并且要提供其需要的参数。一般形式为：

```
function-name (parameters)
```
一个函数调用可以作为一个完整的语句，或者作为一个子表达式。下面是一个作为完整语句的例子：

```
foo (5);
```
这个例子中，函数 `foo` 被调用，并传给了参数 `5`。

下面是一个作为子表达式的例子：

```
a = square (5);
```

假设函数 `square` 平方了它的参数，上面的例子会把值 25 赋给 `a`。

如果参数列表不只一个参数，你要用逗号分割：

```
a = quux (5, 10);
```

### 5.4 函数参数 { #Function-Parameters }

函数参数可以是任何表达式 -- 一个常量值、一个存储在变量中的值、一个内存地址或者由它们构成的复杂表达式。

在函数体内，这些参数是你传进去的值的局部拷贝；你不能通过改变该局部拷贝来修改你传过去的值。

```
int x = 23;
foo (x);
…
/* Definition for function foo. */
int foo (int a)
{
    a = 2 * a;
    return a;
}
```

这个例子中，尽管参数 `a` 在函数 `foo` 中被修改，但你传进去变量 `x` 的值并没有修改。如果你希望修改 `x` 的原始值，那你必须把函数调用写进赋值语句：

```
x = foo (x);
```

如果你传进去的是一个内存地址（也就是一个指针），那么你可以访问（和修改）存在该地址的数据。这相当于其他语言中的引用传递，但并不相同：内存地址只是一个值，就像其他值一样，它自己是不能被修改的。传一个指针和传一个整数的差别在于你想要在函数中做什么。

下面的例子使用指针作为参数来调用函数：

```
void
foo (int *x)
{
    *x = *x + 42;
}
…
int a = 15;
foo (&a);
```

这个函数的形式参数是指向 `int` 类型的指针。我们调用这个函数要传给它一个 `int` 类型变量的地址。在函数用解引用这个指针，我们可以查看和修改存储在该地址的值。上面就把 `a` 的值修改为了 `57`。

即使你不想改变存储在一个地址的值，如果你要传递的变量类型非常大，通常传递指针就比传递变量本身有用，尤其在你需要节省内存或者减少参数拷贝的消耗。例如：

```
struct foo
{
    int x;
    float y;
    double z;
};

void bar (const struct foo *a);
```

在这个例子中，除非你工作的计算机内存很大（不在乎内存消耗），传递指向该结构体实例的指针使用的内存要比传统结构体实例本身要少得多。

数组总是会作为指针传递。

```
void foo (int a[]);
…
int x[100];
foo (x);
```

这个例子中，调用带有参数 `a` 的函数 `foo` 不会复制整个数组到 `foo` 里面的局部参数，而是将 `x` 作为指向第一个参数 `x` 的指针传过去。要注意，你在函数里面不能使用 `sizeof` 来求数组 `x` 的大小，因为 `sizeof` 会告诉你指针 `x` 的大小。实际上，上面的代码等价于：

```
void foo (int *a);
…
int x[100];
foo (x);
```

明确地在参数中指明数组的长度视没有用的。如果你真的想要把数组作为值传递，你可以把它包裹在结构体里，尽管这样做没啥用（传递一个 `const` 修饰的指针通常足以说明调用者不应该修改该数组）。

### 5.5 可变参数列表 { #Variable-Length-Parameter-Lists }

你可以定义一个接受可变参数数量的函数；这种函数被称作 *可变函数*。要定义这样的函数，需要至少一个已知类型的参数，但是其他参数是可选的；并且数量和数据类型都是可变的。

你先正常列出初始参数，随后使用一个省略号 `...`。下面是其原型：

```
int add_multiple_values (int number, ...);
```

要使其工作，需要用到库头文件 `<stdarg.h>` 中定义的宏，因此你必须包含它。要获得这些宏的详细描述，详见 *The GNU C Library* 手册关于可变函数的章节。

举例如下：

```
int
add_multiple_values (int number, ...)
{
    int counter, total = 0;
      
    /* Declare a variable of type ‘va_list’. */
    va_list parameters;
    
    /* Call the ‘va_start’ function. */
    va_start (parameters, number);
    
    for (counter = 0; counter < number; counter++)
    {
        /* Get the values of the optional parameters. */
        total += va_arg (parameters, int);
    }
    
    /* End use of the ‘parameters’ variable. */
    va_end (parameters);
    
    return total;
}
```

要使用可变参数，你需要知道它们有多少个。因为是可变的，所以不能硬编码，但是如果你不知道有多少个参数，那你就很难知道什么时候停止使用 `va_arg` 函数。在上面的例子中，函数 `add_multiple_values` 的第一个参数 `number` 是实际传过来的可变参数的个数。因此我们可以像下面这样调用它：

```
sum = add_multiple_values (3, 12, 34, 190);
```

第一个参数代表了它后面有多少可变参数。

此外要注意，你实际不需要使用 `va_end` 函数。实际上，GCC 中这个函数根本啥也不做。然而，使用它可以最大化地兼容其他编译器。

参见 *The GNU C Library Reference Manual* 中的 [Variadic Functions](http://www.gnu.org/software/libc/manual/html_mono/libc.html#Variadic-Functions) 章节。

### 5.6 通过函数指针调用函数 { #Calling-Functions-Through-Function-Pointers }

你可以通过函数指针来调用函数。这时解引用运算符 `*` 是可选的。

```
#include <stdio.h>

void foo (int i)
{
    printf ("foo %d!\n", i);
}
void bar (int i)
{
    printf ("%d bar!\n", i);
}
void message (void (*func)(int), int times)
{
    int j;
    for (j=0; j<times; ++j)
    func (j);  /* (*func) (j); would be equivalent. */
}

void example (int want_foo) 
{
    void (*pf)(int) = &bar; /* The & is optional. */
    if (want_foo)
        pf = foo;
    message (pf, 5);
}
```
### 5.7 main 函数 { #The-main-Function }

每一个程序都至少需要的一个函数叫做 `main` 函数。它是程序执行的入口。你不需要为 `main` 写声明或原型，但是你要定义它。

`main` 函数的返回值类型必须是 `int`。你可以不必为 `main` 函数指定返回值类型。然而，如果要指定只能是 `int`。

通常，`main` 的返回值代表程序的退出状态。值 `0` 或者 `EXIT_SUCCESS` 代表成功；`EXIT_FAILURE` 代表错误。此外，该返回值的意义由实现定义。

直到 `main` 函数结尾的 `}` 都没有 `return` 语句，或者执行了一个没有返回值的 `return` 语句（也就是 `return;`）是一样的。在 C89 中，结果是未定义的，但在 C99 中等效于 `return 0;`。

你的 `main` 可以不带参数（也就是 `int main(void)`），或者要从命令行接收参数。下面的例子是没有参数的：

```
int
main (void)
{
    puts ("Hi there!");
    return 0;
}
```

要从命令行接收参数，你的 `main` 要有 2 个参数，`int argc` 和 `char *argv[]`。名字可以变，但类型一定要是 `int` 类型和指向 `char` 类型的指针数组。`argc` 是从命令行接收的参数的个数，包含程序名字本身。`argv` 是参数数组，参数是字符串。数组的第一个元素 `argv[0]` 是你在命令行中输入的程序的名字[^4]；后面的数组元素是跟在程序名字后面的参数。

下面是一个接收命令行参数的例子，会把这些参数打印出来：

```
int
main (int argc, char *argv[])
{
    int counter;
    
    for (counter = 0; counter < argc; counter++)
        printf ("%s\n", argv[counter]);
      
    return 0;
}
```


### 5.8 递归函数 { #Recursive-Functions }

你可以定义递归性的函数，也就是函数自己调用自己。这里有一个计算整数阶乘的例子：

```
int
factorial (int x)
{
    if (x < 1)
        return 1;
    else
        return (x * factorial (x - 1));
}
```

注意，你不要写一个无限递归的函数。上面的例子中，一旦 `x` 为 1，递归就结束了。然而在下面的例子中，递归不会停止，直到程序被终止或者内存被耗尽：

```
int
watermelon (int x)
{
    return (watermelon (x));
}
```

函数当然也可以间接地递归。


### 5.9 静态函数 { #Static-Functions }

如果你想定义一个只在函数的源文件中使用的函数，你可以把它定义成静态的：

```
static int
foo (int x)
{
    return x + 42;
}
```

如果你要构建一个可重用的函数库，其中包含一些不希望终端用户调用的子例程，这将非常有用。

这样定义的函数被说成是 *静态连接的*。不幸的是，`static` 这个关键字具有多重含义；参见 [存储类别说明符](Data-Types.md#Storage-Class-Specifiers)

### 5.10 嵌套函数 { #Nested-Functions }

作为 GNU C 扩展，你可以在一个函数里面定义函数，也就是嵌套函数。

下面的例子就是使用嵌套函数定义的尾递归阶乘函数：

```
int
factorial (int x)
{
    int
    factorial_helper (int a, int b)
    {
        if (a < 1)
        {
            return b;
        }
        else
        {
            return factorial_helper ((a - 1), (a * b));
        }
    }
    
    return factorial_helper (x, 1);
}
```

要注意的是，嵌套函数一定要和变量声明一起写在函数开头，所有的其他语句跟在后面。

[^4]: 在极少数情况下，`argv[0]` 可以是空指针（这时 `argc` 是 0）；或者 `argv[0][0]` 可以是空指针。但不管怎样 `argv[argc]` 一定是空指针。
