### 2.1 基础数据类型 { #Data-Types }

#### 2.1.1 整型 { #Integer-Types }

整型的范围在 8 位到 32 位之间。C99 扩展到了 64 位。你应该使用整型来存储整数（并且使用 `char` 类型存储字符）。以下列出的所有整型的大小和范围都是最小的；根据你的计算机平台，他们的大小和范围可能更大。

尽管这些整型的范围很自然地排列[译注]，但标准并不要求任何 2 种类型的范围不同。例如，众所周知 `int` 和 `long` 的范围是相同的。标准甚至允许 `singed char` 和 `long`  范围相同，尽管这样的平台很少见。

- `singed char`

    8 位 `signed char` 类型能放得下范围在 -128 和 127 之间的整数
  
- `unsigned char`

    8 位 `unsigned char` 类型能放得下范围在 0 和 255 之间的整数

- `char`

    根据你的系统不同，`char` 类型要么和 `signed char` 相同，要么和 `unsigned char` 相同（然而他们是 3 种不同的类型）。按照惯例，你应该使用 `char` 类型来存储 ASCII 字符（如 `'m'`），包括转义字符（如 `'\n'`）
  
- `short int`

    16 位 `short int` 类型能放得下范围在 -32768 和 32767 之间的整数。你也可以写成 `short`, `signed short int` 或者 `signed short`

- `unsigned short int`

    16 位 `unsigned short int` 类型能放得下范围在 0 和 65535 之间的整数。你也可以写成 `unsigned short`

- `int`

    32 位 `int` 类型能放得下范围在 -2147483648 和 2147483647 之间的整数。你也可以写成 `signed int` 或者 `signed`
  
- `unsigned int`

    32 位 `unsigned int` 类型能放得下范围在 0 和 4294967295 之间的整数。你也可以简写成 `unsigned`

- `long int`

    32 位 `long int` 类型至少能放得下范围在 -2147483648 和 2147483647 之间的整数（根据你的系统不同，该数据类型可能是 64 位的，这样的话它就相当于 `long long int `类型了）。你也可以写成 `long`，`signed long int`，或者 `signed long`

- `unsigned long int`

    32 位 `unsigned long int` 类型至少能放得下范围在 0 和 4294967295 之间的整数（根据你的系统不同，该数据类型可能是 64 位的，这样的话它就相当于 `unsigned long long int` 类型了）。你也可以写成 `unsigned long`

- `long long int`

    64 位 `long long int` 类型能放得下范围在 -9223372036854775808 和  9223372036854775807 之间的整数。你也可以写成 `long long`，`signed long long int` 或者 `signed long long`。这个类型不属于 C89 的一部分，但属于 C99 和 GNU C 扩展
  
- `unsigned long long int`

    64 位 `unsigned long long int` 类型能放得下范围在 0 和 18446744073709551615 之间的整数。你也可以写成 `unsigned long long`。这个类型不属于 C89 的一部分，但属于 C99 和 GNU C 扩展

下面是几个声明和定义整型的例子：

```
int foo;
unsigned int bar = 42;
char quux = 'a';
```

第一行声明了一个叫做 `foo` 的整数，但是没有给他定义值；它是未初始化的，所以你通常不能假设任何值。

#### 2.1.2 实数类型 { #Real-Number-Types }

共有 3 种数据类型来表示分数。尽管在现在使用的大多数计算机系统上它们的大小和范围都是一样的，但在历史上不同的系统却是不同的。因此最大值和最小值以宏定义的形式被存在库头文件 `float.h` 中。这一部分，我们用宏定义的名字来代替它们的可能值；你可以查看你系统的 `float.h` 来或者明确地值。

- `float`

    `float` 类型是 3 种浮点类型中最小的，如果他们的大小根本不同的话。它的最小值存在 `FLT_MIN` 中，并应该不大于 `1e-37`；最大值存在 `FLT_MAX` 中，并应该不小于 `1e37`

- `double`

    `double` 类型至少要和 `float` 类型一样大，并应该更大。它的最小值存在 `DBL_MIN `中；最大值存在 `DBL_MAX` 中
  
- `long double`

    `long double` 类型至少要和 `float` 类型一样大，并应该更大。它的最小值存在 `LDBL_MIN` 中；最大值存在 `LDBL_MAX` 中

所有浮点类型都是有符号的；使用 `unsigned float` 会造成编译时错误的。

这里有几个声明和定义实数类型变量的例子：

```
float foo;
double bar = 114.3943;
```

第一行声明了一个叫做 `foo` 的 float 类型变量但未定义值；它是未初始化的，所以你通常不能假设任何值。

C 中提供的实数类型精度有限，所以并不能精确表示所有的实数。大多数使用 GCC 的计算机系统使用二进制表示实数，因此不能表示像 `4.2` 这样的数。为此，我们推荐你在判实数相等的时候不要使用 == 操作符，而是检查 2 个实数的差是否在可接受的范围内。

此外还有一个由于不精确表示造成的些许兼容性问题。详情参见 David Goldberg 的论文 *What Every Computer Scientist Should Know About Floating-Point Arithmetic* 以及 Donald Knuth 的 *The Art of Computer Programming* 的 4.2.2 节。

#### 2.1.3 复数类型 { #Complex-Number-Types }

GCC 引入一些复数类型作为 C89 的扩展。类似的特性也被引入 C99[^1]，但它们有一些不同。我们先介绍标准的复数类型。

##### 2.1.3.1 标准复数类型 { #Standard-Complex-Number-Types }

Complex 类型在 C99 中被引入。共有 3 种复数类型：

``` c
float _Complex
double _Complex
ong double _Complex
```

这里的名字都是以一个下划线跟一个大写字母开始，以免和现有程序的标识符冲突。然而，C99 的标准头文件 `<ltcomplex.h>` 引入了一些宏来简化复数类型的使用。

- `complex`

    代替 `_Complex`。使得可以这样 `double complex` 声明变量，看起来更自然一些。

- `I`

    `I` 是 `const float _Complex` 类型的常量，值是复数的虚部单位，通常说的 *i*。

`<complex.h>` 也声明了一些复数运算函数，如 `creal` 和 `cimag` 函数分别用来获取 `double complex` 类型复数的实部和虚部。还有其他函数，像下面例子所示的：

```
#include <complex.h>    
#include <stdio.h>  

void example (void) 
{    
    complex double z = 1.0 + 3.0*I; 
    printf ("Phase is %f, modulus is %f\n", carg (z), cabs (z));        
}  
```

##### 2.1.3.2 GNU 扩展的复数类型 { #GNU-Extensions-for-Complex-Number-Types }

GCC 也引入了复数类型作为 C89 的 GNU 扩展，但是写法不同。在 C89 的 GCC 扩展中，浮点复数的写法如下：

```
__complex__ float
__complex__ double
__complex__ long double
```

GCC 扩展不止允许复数是浮点类型，因此你也可以声明字符复数类型和整数复数类型；实际上 `__complex__` 可以用在任何基础数据类型上。这里不会给出所有的组合，但会给几个例子：

- `__complex__ float`

    `__complex__ float` 类型有 2 部分：实部和虚部，他们都是 `float` 类型
  
- `__complex__ int`

    `__complex__ int` 也有 2 部分：实部和虚部，它们都是 `int` 类型

为了获取一个复数值表达式的实部，你可以使用 `__real__` 关键字，后面跟上该表达式；同样的，可以使用 `__imag__` 获取虚部。

```
__complex__ float a = 4 + 3i;

float b = __real__ a;          /* b is now 4. */
float c = __imag__ a;          /* c is now 3. */
```

这个例子声明了一个浮点复数类型的变量 `a`，它的实部是 4，虚部是 3。然后将其实部赋给了浮点变量 `b`，将虚部赋给了浮点变量 `c`。


### 2.2 枚举类型 { #Enumerations }

枚举是用来存储整型常量并通过名字引用他们的自定义类型。默认情况下，这些值的类型是 `signed int`；然而你可以使用 GCC 编译选项 `-fshort-enums` 将其变为可能最小的整数类型。这些行为同样适用于 C89 标准，但在同一程序里混合使用这些选项可能会造成兼容性问题。

#### 2.2.1 定义枚举 { #Defining-Enumerations }

你可以使用 `enum` 关键字来定义枚举，后面跟上枚举的名字（这是可选的），然后跟上一列常量名字（使用逗号分隔并用花括号括起来），最后以分号结束。

```
enum fruit {grape, cherry, lemon, kiwi};
```

这个例子中定义了一个枚举 `fruit`，它包含了 4 个整型常量值 `grape`、`cherry`、 `lemon` 和 `kiwi`，它们的值分别为默认值 0、1、2 和 3。你也可以为其中一个或多个指定值：

```
enum more_fruit {banana = -17, apple, blueberry, mango};
```

这个例子中，定义 `banana` 为 -17，剩余的值会自动加 1：`apple` 是 -16， `blueberry` 是 -15，`mango` 是 -14。除非指定，否则一个枚举类型的值是上一个值加 1（第一个值默认为 0）。

你也可以用同一个枚举中之前定义的值来定义新的值：

```
enum yet_more_fruit {kumquat, raspberry, peach,
                     plum = peach + 2};
```

在上面例子中，`kumquat` 是 0，`raspberry` 是 1，`peach` 是 2，`plum` 是 4。

你不能在同一个作用域中定义相同名字的 `enum`、`struct` 和 `union`。

#### 2.2.2 声明枚举 { #Declaring-Enumerations }

你既可以在定义枚举类型的时候声明一个枚举变量，也可以之后再声明。下面例子在一个语句中声明了一个叫 `my_fruit` 的 `enum fruit` 类型的变量：

```
enum fruit {banana, apple, blueberry, mango} my_fruit;
```

而这个例子把类型声明和变量声明分开了：

```
enum fruit {banana, apple, blueberry, mango};
enum fruit my_fruit;
```

（当然，如果你没有命名这个枚举类型，就不能用这种方式了）    

尽管这些变量被当作枚举类型，但你可以给他们赋值任何可以赋值给 `int` 变量的值，包括从其他枚举变量赋值。换句话说，任何可以赋值 `int` 值的变量都能被赋值给一个枚举类型。

然而，一旦定义了一个枚举后，你就不能再改变他的值；它们是常量。例如，这不能正常工作：

```
enum fruit {banana, apple, blueberry, mango};
banana = 15;  /* You can’t do this! */
```

枚举连同 `swich` 语句一起很有用，因为如果你少处理一个枚举值编译器就会警告你。那上面的例子来说，如果你的代码仅仅处理了 `banana`、`apple` 和 `mango` 但没有处理 `blueberry`，GCC 会警告你。

### 2.3 联合体 { #Unions }

联合体是自定义数据类型，被用来在同一块儿内存空间存储多个变量。尽管你可以在任何时候访问联合体中的任何变量，但你应该一次仅仅读一个值 -- 给一个变量赋值会覆盖其他的变量。

#### 2.3.1 定义联合体 { #Defining-Unions }

你可以使用 `union` 关键字来定义联合体，后面跟上用花括号括起来的该联合体的成员声明。声明每一个联合体成员就像平时声明变量一样 -- 使用数据类型后跟一个或多个变量名字并用逗号分割的形式末尾跟上一个分号。联合体定义结束要在右花括号后面加上分号。

你也可以给联合体加上一个名字，放在 `union` 关键字和左花括号中间。这在语法上是可选的，但是如果你不加，随后你就没办法使用该联合体了（没有 `typedef` 的话，详见 [typedef 语句](Statement.md#The-typedef-Statement)）。

下面是一个例子，定义了一个放整数值和浮点数值的简单联合体：

```
union numbers
{
    int i;
    float f;
};
```
 
该例子定义了一个叫 `numbers` 的联合体，它包含 2 个成员 `i` 和 `f`，分别是 `int` 类型和 `float` 类型。

#### 2.3.2 声明联合体变量 { #Declaring-Union-Variables }

你可以在初始定义联合体时声明该联合体类型的变量，也可以在定义之后，如果你给该联合体类型命名的话。

##### 2.3.2.1 定义时声明联合体变量 { #Declaring-Union-Variables-at-Definition }

你可以在定义联合体时声明该联合体类型的变量。方法是把变量名放在联合体声明的右花括号之后，但在最后的分号之前。你可以声明不止一个变量，要将变量名用逗号分开。

```
union numbers
{
    int i;
    float f;
} first_number, second_number;
```

这个例子声明了 2 个 `union numbers` 类型的变量 `first_number` 和 `second_number`。

##### 2.3.2.2 定义后声明联合体变量 { #Declaring-Union-Variables-After-Definition }

你可以在定义联合体后声明该联合体类型的变量。方法是使用 `union` 关键字和你给该联合体类型起的名字，跟上一个或多个逗号分割的变量名字。

```
union numbers
{
    int i;
    float f;
};
union numbers first_number, second_number;
```

这个例子声明了 2 个 `union numbers` 类型的变量 `first_number` 和 `second_number`。

##### 2.3.2.3 初始化联合体成员 { #Initializing-Union-Members }

你可以在声明一个联合体变量时就初始化第一个成员：

```
union numbers
{
int i;
float f;
};
union numbers first_number = { 5 };
```

这个例子中，`first_number` 的成员 `i` 给了值 5。而成员 `f` 没有赋值。

其他初始化联合体成员的方法是制定该成员的名字。这样你就可以初始化你想初始化的成员，而不是仅仅初始化第一个成员。有 2 种方法可以使用--一种是在成员后面跟一个冒号，然后是要赋的值：

```
union numbers first_number = { f: 3.14159 };
```

或者在成员名字前面加上一个点，然后用赋值操作符赋一个值：

```
union numbers first_number = { .f = 3.14159 };
```

你也可以在定义时声明联合体变量时初始化一个联合体成员：

```
union numbers
{
int i;
float f;
} first_number = { 5 };
```

#### 2.3.3 访问联合体成员 {#Accessing-Union-Members}

你可以通过成员访问操作符来访问一个联合体变量的成员。变量名放操作符左边，成员名放操作符右边。

```
union numbers
{
int i;
float f;
};
union numbers first_number;
first_number.i = 5;
first_number.f = 3.9;
```

要注意的是，在上面例子中给成员 `f` 赋值之后会覆盖成员 `i` 的值。

#### 2.3.4 联合体的大小 { #Size-of-Unions }

联合体的大小等于它最大成员的大小。考虑本节中的例子：

```
union numbers
{
int i;
float f;
};
```

这个联合体类型的大小等于 `sizeof(float)`，因为 `float` 类型比 `int` 类型要大。既然联合体的所有成员都占用同一块儿内存空间，所以联合体类型不需要大到足够放下所有成员；它只需要足够放下最大的成员。


### 2.4 结构体 { #Structures }

结构体是程序员定义的数据类型，由其他数据类型的变量组成（也有可能包含其他结构体类型）。

#### 2.4.1 定义结构体 { #Defining-Structures }

你可以使用 `struct` 关键字后跟用花括号括起来的结构体成员声明来定义一个结构体。在声明每一个结构体成员的时候就像声明普通变量一样--数据类型后跟一个或多个用逗号分割的变量名，末尾以分号结束。结构体定义要在右花括号后加分号结束。

你也可以在 `struct` 关键字和左括号之间加一个结构体名字。这是可选的，但是如果你不加，在定义之后你就无法使用该结构体类型了（在不使用 `typedef` 的情况下，详见 [typedef 语句](Statement.md#The-typedef-Statement)

这里有一个定义简单结构体的例子，放了一个点的 X 和 Y 坐标：

```
struct point
{
int x, y;
};
```

它定义了一个叫做 `struct point` 的结构体，包含 2 个成员 `x` 和 `y`，都是 `int` 类型。

结构体（和联合体）可能包含其他结构体和联合体，但不能包含它们自身。但对于一个结构体或联合体来说包含一个指向相同类型的指针域是可以的（详见 [不完整类型](#Incomplete-Types)。

#### 2.4.2 声明结构体变量 { #Declaring-Structure-Variables }

你可以在初始定义一个结构体类型时声明该结构体类型变量，也可以在定义之后，如果你给该结构体类型命名的话。

##### 2.4.2.1 定义时声明结构体变量 { #Declaring-Structure-Variables-at-Definition }

在定义一个结构体类型时声明该结构体类型变量，你需要把变量名放在结构体类型定义的右花括号之后，但在最后的分号之前。你可以使用逗号分割的形式来声明不止一个变量。

```
struct point
{
int x, y;
} first_point, second_point;
```

这个例子声明了 2 个 `struct point` 类型变量 `first_point` 和 `second_point`。

##### 2.4.2.2 定义后声明结构体变量 { #Declaring-Structure-Variables-After-Definition }

你也可以在结构体类型定义之后声明该结构体变量。你需要使用 `struct` 关键字以及你给结构体类型起的名字，后跟一个或多个用逗号分割的变量名。

```
struct point
{
int x, y;
};
struct point first_point, second_point;
```

这个例子声明了 2 个 `struct point` 类型变量 `first_point` 和 `second_point`。

##### 2.4.2.3 初始化结构体成员 { #Initializing-Structure-Members }

你可以在声明结构体变量时就给结构体类型的成员初始化确定的值。

如果你没有初始化结构体变量，结果依赖于该变量是否是静态存储的（详见 [存储类别说明符](#Storage-Class-Specifiers)）。如果是，数字类型的成员被初始化为 0，指针类型的成员被初始化为 NULL；否则，结构体成员的值是不确定的。

一种初始化结构体的方法是在花括号中指定值，并用逗号分割。这些值按照和结构体定义相同的顺序赋值给结构体的成员。

```
struct point
{
int x, y;
};
struct point first_point = { 5, 10 };
```

这个例子中，`first_point` 中的 `x` 成员赋值了 5， `y` 成员赋值了 10.

其他初始化成员的方式是指定你要初始化的成员名字。这种方式下，你可以以任意顺序初始化，甚至可以留一些不初始化。有 2 种方式你可以使用。第一种在 C99 中可用，另一种是 GCC 的 C89 扩展：

```
struct point first_point = { .y = 10, .x = 5 };
```

你可以省略掉点号并使用冒号而不是等号，尽管这是 GNU C 的扩展：

```
struct point first_point = { y: 10, x: 5 };
```

你也可以在定义结构体期间进行变量声明时初始化结构体变量：

```
struct point
{
int x, y;
} first_point = { 5, 10 };
```

你不必初始化所有结构体变量的成员：

```
struct pointy
{
int x, y;
char *p;
};
struct pointy first_pointy = { 5 };
```

这里的话， `x` 被初始化为 5， `y` 初始化为 0，而 `p` 被初始化为 NULL。这个规则就像 `y` 和 `p` 是静态变量一样。

下面的例子是初始化一个结构体成员，而该成员也是结构体：

```
struct point
{
int x, y;
};

struct rectangle
{
struct point top_left, bottom_right;
};

struct rectangle my_rectangle = { {0, 5}, {10, 0} };
```

这个例子中，定义了一个叫做 `rectangle` 的结构体包含 2 个 `point` 结构体变量。然后声明了一个 `struct rectangle` 类型的结构体变量并初始化它的成员。既然它的成员是结构体变量，我们得再使用一个花括号来括住 `point` 结构体的成员。然而，该花括号不是必须的，这样做仅仅使代码易读。

#### 2.4.3 访问结构体成员 { #Accessing-Structure-Members }

你可以使用成员访问操作符来访问结构体变量的成员。你要把结构体变量的名字放在操作符左边，把成员名字放在操作符右边。

```
struct point
{
int x, y;
};

struct point first_point;

first_point.x = 0;
first_point.y = 5;
```

你也可以访问一个结构体变量的成员，而该结构体变量是其他结构体变量的成员。

```
struct rectangle
{
struct point top_left, bottom_right;
};

struct rectangle my_rectangle;

my_rectangle.top_left.x = 0;
my_rectangle.top_left.y = 5;

my_rectangle.bottom_right.x = 10;
my_rectangle.bottom_right.y = 0;
```

#### 2.4.4 位域 { #Bit-Fields }

你可以使用非标准大小的整型来创建结构体，称之为*位域*。要使用位域，你可以像通常那样指定一个整型成员（`int`，`char`，`long int` 等），并在成员名字和分号之间插入一个冒号和该成员占的位数。

```
struct card
{
unsigned int suit : 2;
unsigned int face_value : 4;
};
```

这个例子定义了一个结构体类型，它有 2 个位域 `suit` 和 `face_value`，分别占 2 个位和 4 个位。`suit` 能放 0~3，而 `face_value` 能放 0~15。注意这些位域被声明为了 `unsigned int` 类型；如果它们被声明为 `signed int` 类型，那它们的值范围将分别是 -2~1 和 -8~7。

通常情况下，*N* 位 unsigned 的位域范围是 0 到 *2^N-1*，而 *N* 位 *signed* 的位域范围是 *-(2^N) / 2* 到 *((2^N) / 2) -1*。

位域可以不要名字，以便控制容纳单元中哪些实际的位被使用。然而，这种方式的结果是不可移植的，所以很少使用。你也可以指定一个位域的大小为 0，这意味着该位域后面的位域应该打包进前面的位域中。这通常也没啥用。

你不能对位域使用地址操作符 `&` 来对其取地址（详见 [指针操作符](Expressions-And-Operators.md#Pointer-Operators)）

#### 2.4.5 结构体大小 { #Size-of-Structures }

结构体类型的大小等于其所有成员的大小之和，可能包括使结构类型与特定字节边界对齐的填充。对齐细节会根据你的计算机平台而不同，但一般会看到对 4 字节或 8 字节边界对齐。这样做是为了加速结构体类型实例的内存访问。

GNU 扩展允许结构体没有任何成员，这样的结构体大小为 0.

如果你明确不要填充你的结构体类型（这样做反而会降低对结构体的访存速度），GCC 提供了几种关闭填充的方法。较快且易用的方式是使用 `-fpack-struct` 编译选项。更多关于省略填充的细节，请参考你使用的编译器版本的对应手册。


### 2.5 数组 { #Arrays }

数组这种数据结构能让你在内存中连续存储一个或多个元素。在 C 中，数组元素的索引从 0 开始，而不是 1.

#### 2.5.1 声明数组 { #Declaring-Arrays }

要声明数组，你可以通过指定它的元素的数据类型，它的名字以及它能存储多少元素。下面是一个声明了可以存放 10 个整数的数组的例子：

```
int my_array[10];
```

对于标准 C 代码，元素的个数只能是正数。

GNU 扩展允许元素的个数可以小到 0. 0 长度的数组作为结构体的最后一个元素很有用，这样的结构体可以作为一个可变长度对象的头部。

```
struct line
{
int length;
char contents[0];
};

{
struct line *this_line = (struct line *)
malloc (sizeof (struct line) + this_length);
this_line -> length = this_length;
}
```

其他的 GNU 扩展允许你使用变量来声明一个数组的大小，而不仅仅是常量。例如，下面这个函数定义中，使用它的参数作为元素个数来声明一个数组：

```
int
my_function (int number)
{
int my_array[number];
…;
}
```


#### 2.5.2 初始化数组 { #Initializing-Arrays }

你可以在声明数组的时候通过花括号列出逗号分割的初始化值来初始化数组的元素。例如：

```
int my_array[5] = { 0, 1, 2, 3, 4 };
```

你不必完全初始化所有的元素。例如，下面的代码初始化了前面 3 个元素，后面 2 个被初始化了默认值 0.

```
int my_array[5] = { 0, 1, 2 };
```

在使用 ISO C99 或者 GNU 扩展的 C89 时，你也可以不按顺序初始化，而是通过指定数组下标。要这样做，你得用方括号括起来的下标，并且后面（要初始化的值前面）可以跟一个可选的赋值操作符。例如：

```
int my_array[5] = { [2] 5, [4] 9 };
```

或者使用赋值操作符：

```
int my_array[5] = { [2] = 5, [4] = 9 };
```

这 2 种方式和下面的例子是等价的：

```
int my_array[5] = { 0, 0, 5, 0, 9 };
```

在使用 GNU 扩展的时候，你还可以给一列连续的元素赋给相同的值，只要指定第一个和最后一个元素的下标，形式如 `[first] ... [last]`。例如：

```
int new_array[100] = { [0 ... 9] = 1, [10 ... 98] = 2, 3 };
```

这将元素 0 到 9初始化为 1，10 到 98 初始化为 2，元素 99 初始化为 3。（你当然也可以明确得写成 [99] = 3）。此外，你要注意在 ... 的两边都有空格。

如果你初始化数组的所有元素，则你不必指定它的大小了；它的大小会根据你初始化的元素的个数而决定。例如：

```
int my_array[] = { 0, 1, 2, 3, 4 };
```

尽管这里没有用 `my_array[5]` 来明确指明该数组有 5 个元素，但它初始化了 5 个元素，因此该数组就有 5 个元素。

此外，如果你通过指定的方式初始化数组元素，那该数组的大小就是你初始化的最高的元素下标加上 1。例如：

```
int my_array[] = { 0, 1, 2, [99] = 99 };
```

这个例子中，虽然只初始化了 4 个元素，但最后一个初始化的元素的下标是 99，因此该数组有 100 个元素。

#### 2.5.3 访问数组 { #Accessing-Array-Elements }

你可以通过指定数组名并在后面跟上用方括号括起来的元素下标来访问该元素。记得，数组下标从 0 开始。例如：

```
my_array[0] = 5;
```

这个例子将值 5 赋给了数组的第一个元素，该元素在位置 0 上。你可以把一个数组元素当做一个变量，该变量的数据类型就是其数组的类型。例如，如果你有一个结构体类型的数组，你可以像下面那样访问结构体的元素：

```
struct point
{
int x, y;
};
struct point point_array[2] = { {4, 5}, {8, 9} };
point_array[0].x = 3;
```


#### 2.5.4 多维数组 { #Multidimensional-Arrays }

你可以造出多维数组，或者叫“数组的数组”。只要再加上一个方括号以及一个数组长度代表新维度的每一个数组的长度。例如，这里声明了一个有 2 个元素的数组，每一个元素是一个拥有 5 个元素的数组：

```
int two_dimensions[2][5] { {1, 2, 3, 4, 5}, {6, 7, 8, 9, 10} };
```

多维数组元素的访问要指定每一维的下标：

```
two_dimensions[1][3] = 12;
```

在这个例子中，`two_dimensions[0]` 本身是一个数组。元素 `two_dimensions[0][2]` 后面是 `two_dimensions[0][3]` 而不是 `two_dimensions[1][2]`。

#### 2.5.5 作为字符串的数组 { #Arrays-as-Strings }

你可以用一个字符数组来存放一个字符串（详见 [字符串常量](Lexical-Elements.md#String-Constants)）。该数组应该是有符号或者无符号的字符类型。

当你声明数组时你可以指定数组大小。该大小是数组能存放的最大字符数，包括字符串结尾的 null 字符。如果你指定了大小，那就不必在声明时初始化它。此外，你可以简单地初始化数组，如果数组的大小足够存放你初始化的字符串。

有 2 种方式初始化数组。你可以指定花括号括起来的一列逗号分割的字符，或者指定双引号引起来的字符串常量。

例如：

```
char blue[26];
char yellow[26] = {'y', 'e', 'l', 'l', 'o', 'w', '\0'};
char orange[26] = "orange";
char gray[] = {'g', 'r', 'a', 'y', '\0'};
char salmon[] = "salmon";
```

在这些例子中，末尾都加了 null 字符，即使你没有显示地写明。（注意，如果你用字符的形式初始化数组，那就必须显示添加 null 字符。虽然在某些特殊情况会意外存在末尾的 null 字符，但你不能以来这种偶然）

初始化后你就不能再用赋值操作符来赋值一个字符串常量了。例如，下面的代码不能工作：

```
char lemon[26] = "custard";
lemon = "steak sauce";      /* Fails! */
```

然而，GNU C 库中有一些函数可以操作字符串（包括拷贝）。你也可以像使用其他类型数组一样，通过访问字符串
各个元素的方式来一次修改一个字符：

```
char name[] = "bob";
name[0] = 'r';
```

有时候你可能指定了数组大小，但给它初始化时却用了一个比它长的字符串。这不是个好事情。该字符串并不能取代这个数组，并且你会得到一个编译时警告。因为原来的数组大小固定，则字符串的超出部分会写到不属于这个数组的内存空间。


#### 2.5.6 联合体数组 { #Arrays-of-Unions }

你可以创建一个联合体类型的数组，就像创建基础类型数组一样。

```
union numbers
{
int i;
float f;
};
union numbers number_array [3];
```

这个例子创建了一个 3 个联合体变量的数组，名字叫 `number_array`。你可以这样初始化每一个联合体的第一个成员：

```
union numbers number_array [3] = { {3}, {4}, {5} };
```

内部的分组花括号是可选的。

在初始化后，你仍可以使用成员访问操作符来访问数组中的联合体成员。操作符的左边是数组名和数组元素下标（用方括号括起来），右边是成员名字。

```
union numbers number_array [3];
number_array[0].i = 2;
```


#### 2.5.7 结构体数组 { #Arrays-of-Structures }

你可以创建一个结构体类型的数组，就像创建基础类型数组一样。

```
struct point
{
int x, y;
};
struct point point_array [3];
```

这个例子创建了一个 3 个结构体变量的数组，名字叫 `point_array`。你可以这样初始化每一个结构体元素：

```
struct point point_array [3] = { {2, 3}, {4, 5}, {6, 7} };
```

就和初始化包含结构体类型成员的结构体一样，内部的分组花括号是可选的。但是，如果你带上了分组花括号，你可以只初始化某些元素的部分成员。

```
struct point point_array [3] = { {2}, {4, 5}, {6, 7} };
```

这个例子中，第一个元素只初始化了 `x` 成员。因为分组花括号， 值 4 被赋给了第二个元素的 `x` 成员，而不是第一个元素的 `y` 成员。而如果没有分组花括号则相反。

在初始化后，你仍可以使用成员访问操作符来访问数组中的结构体成员。操作符的左边是数组名和数组元素下标（用方括号括起来），右边是成员名字。

```
struct point point_array [3];
point_array[0].x = 2;
point_array[0].y = 3;
```


### 2.6 指针 { #Pointers }

指针用来存放常量或变量的内存地址。对于任何数据类型，包括基础类型和自定义类型，你可以创建一个指针来存放该类型实例的内存地址。

#### 2.6.1 声明指针 { #Declaring-Pointers }

你可以通过指定一个名字和一个数据类型来声明指针。数据类型是指该指针要存放的是哪种类型变量内存地址。

要声明一个指针，在标识符前面加上间接寻址操作符（详见 [指针操作符](Expressions-And-Operators.md#Pointer-Operators)）下面是一般形式：

```
data-type * name;
```

在间接寻址操作符两边的空白符是没有意义的：

```
data-type *name;
data-type* name;
```

下面的例子声明了一个存放 `int` 变量地址的指针：

```
int *ip;
```

要当心的是，在同一个语句中声明多个指针时，一定要在每一个指针前都使用间接寻址操作符：

```
int *foo, *bar;  /* Two pointers. */
int *baz, quux;   /* A pointer and an integer variable. */
```


#### 2.6.2 初始化指针 { #Initializing-Pointers }

你可以在声明指针时通过赋给一个变量地址初始化它。例如，下面的代码声明了一个 `int` 变量 `i`，和一个指针 `ip`，并初始化为了 `i` 的地址：

```
int i;
int *ip = &i;
```

使用地址操作符（详见 [指针操作符](Expressions-And-Operators.md#Pointer-Operators)）来获取变量的地址。在声明一个指针后，要给该指针赋值一个新的地址时就不要在指针的名字前使用间接寻址操作符了。相反，这样做会改变指针指向变量的值，而不是指针它自己的值，例如：

```
int i, j;
int *ip = &i;  /* ‘ip’ now holds the address of ‘i’. */
ip = &j;       /* ‘ip’ now holds the address of ‘j’. */
*ip = &i;      /* ‘j’ now holds the address of ‘i’. */
```

存在指针里面的值是一个整数：一个计算机内存空间的地址。如果你想的话，你可以赋给指针一个数字常量，会转换数字常量为正确的指针类型。然而，我们不推荐这么做，除非你能极好地控制你在内存中存放了什么，并清楚地了解你在做什么。因为这样做很容易不小心覆盖掉你并不像覆盖的内容。并且大多数这样的技术都是不可移植的。

有一点很重要：如果你没有使用已存在的对象的地址来初始化指针，那它通常不指向任何地方；如果你使用它，很可能导致你的程序挂掉（通常这种行为被称作未定义行为）

#### 2.6.3 联合体指针 { #Pointers-to-Unions }

你可以创建一个联合体指针，就像创建其他基础类型指针一样。

```
union numbers
{
int i;
float f;
};
union numbers foo = {4};
union numbers *number_ptr = &foo;
```

上面这个例子创建了一个新的联合体类型 `union numbers`，并声明了一个该类型的变量 `foo`（同时初始化了第一个成员）。最后声明了一个指向该类型的指针，并将 `foo` 的地址赋给了它。

你可以通过指针来访问联合体变量的成员，但你不能像前面那样使用成员访问操作符了。而是要用间接成员访问操作符（详见 [成员访问表达式](Expressions-And-Operators.md#Member-Access-Expressions)）。继续前面的例子，下面的示例将改变 `foo` 变量的第一个成员的值：

```
number_ptr -> i = 450;
Now the i member in foo is 450.
```

#### 2.6.4 结构体指针 { #Pointers-to-Structures }

你可以创建一个结构体类型指针，就像创建其他基础类型指针一样。

```
struct fish
{
float length, weight;
};
struct fish salmon = {4.3, 5.8};
struct fish *fish_ptr = &salmon;
```

上面这个例子创建了一个新的结构体类型 `struct fish`，并声明了一个该类型的变量 `salmon`（同时初始化）。最后声明了一个指向该类型的指针，并将 `salmon` 的地址赋给了它。

你可以通过指针来访问结构体变量的成员，但你不能像前面那样使用成员访问操作符了。而是要用间接成员访问操作符（详见 [成员访问表达式](Expressions-And-Operators.md#Member-Access-Expressions)）。继续前面的例子，下面的示例将改变 `salmon` 变量的成员的值：

```
fish_ptr -> length = 5.1;
fish_ptr -> weight = 6.2;
```

现在 `salmon` 的成员 `length` 和 `width` 的值分别是 5.1 和 6.2 了。


### 2.7 不完整类型 { #Incomplete-Types }

你可以定义结构体，联合体和枚举而不列出他们的成员（在枚举中叫做值）。这样做会产生一个不完整类型。你不能使用不完整类型来声明变量，但你可以使用指向这种类型的指针。

```
struct point;
```

在你程序的后面你会补全这个类型。就像你平常定义一样：

```
struct point
{
int x, y;
};
```

这种技术经常用于链表：

```
struct singly_linked_list
{
struct singly_linked_list *next;
int x;
/* other members here perhaps */
};
struct singly_linked_list *list_head;
```


### 2.8 类型限定符 { #Type-Qualifiers }

有 2 种类型限定可以加在变量声明前面来改变变量的访问：`const` 和 `volatile`。

`const` 把变量变成只读的；初始化之后值就不能再改变。

```
const float pi = 3.14159f;
```

除了防止值意外被改变，声明 `const` 变量还能帮助编译器优化代码。

`volatile` 告诉编译器该变量经常改变，看似无用的变量访问（例如，通过指针）不应该被优化掉。你可能会在回调函数会更新存储数据和信号处理时使用 `volatile` 变量。参见 [序列点和信号传递](Expressions-And-Operators.md#Sequence-Points-and-Signal-Delivery)

```
volatile float currentTemperature = 40.0;
```

### 2.9 存储类别说明符 { #Storage-Class-Specifiers }

有 4 种存储类别可以加在你的变量前面来改变变量在内存中如何存储：`auto`，`extern`，`register` 和 `static`。

使用 `auto` 会将变量存在函数本地，变量的值在函数返回后会被销毁。这是在函数中定义变量的默认行为。

```
void
foo (int value)
{
auto int x = value;
…
return;
}
```

`register` 作用和 `auto` 非常类似，除了它会建议编译器该变量会经常用到，如果可能就把该变量存在寄存器里。你不能使用取地址操作符来获取 `register` 变量的地址。这意味着你不能引用 `register` 数组的元素。实际上，对于这种数组你唯一能做的就是使用 `sizeof` 来获取长度。GCC 通常对哪些值应该放进寄存器会有很好的选择，所以 `register` 不会常用到。

`static` 本质上和 `auto` 是相对的：如果在函数和代码块里使用 `static`，变量会一直保留他们的值，即使函数或代码块结束。这就是静态存储期。

```
int
sum (int x)
{
static int sumSoFar = 0;
sumSoFar = sumSoFar + x;
return sumSoFar;
}
```

你也可以在最外层（也就是不在函数里面）声明 `static` 变量（或函数）；这种变量对当前整个源文件全局可见（但是对其他源文件不可见）。不幸的是，这样 `static` 就有了二义性；第二种含义也叫做*静态连接*。2 个在不同文件的静态函数或变量是完全隔离的；任何一个对除声明它的文件外都不可见。

未初始化的 `extern` 变量会给一个默认值 0, 0.0 或者 NULL，取决于变量是什么类型。未初始化的 `auto` 或 `register` 变量（包括默认的 `auto`）都保持未初始化状态，因此不要假设它的值。

`extern` 对想要声明整个连接的项目可见的变量十分有用。你不能初始化一个 `extern` 声明的变量，因为在声明期间并没有给它分配空间。你必须既要做 `extern` 声明（通常在会被其他源文件包含的用以访问该变量的头文件里）又要做非 `extern` 声明，非 `extern` 声明实际是用来分配存储空间的。 `extern` 声明可以有很多次。

```
extern int numberOfClients;

…

int numberOfClients = 0;
```

程序结构相关信息详见 [程序结构和作用域](Program-Structure-And-Scope.md)。


### 2.10 类型别名 { #Renaming-Types }

有时给类型起一个新名字是很方便的。你可以使用 `typedef` 语句来给类型起别名。详见 [typedef 语句](Statement.md#The-typedef-Statement)。

[^1]: C++ 也支持复数类型，但与 ISO C99 不兼容
