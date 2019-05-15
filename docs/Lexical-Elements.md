本章描述了在预处理之后组成 C 源代码的词汇元素。这些元素被称为 token。共有 5 种类型的 token：关键字、标识符、常量、操作符以及分隔符。空白符，有时候也用来分割 token，会在本章提到。

### 1.1 标识符 { #Identifiers }

标识符是一种为变量、函数、新数据类型以及预处理宏命名的字符序列。你可以在标识符中使用字母、数字和下划线 '_'。

标识符的第一个字符不能是数字。

大小写字母是不同的，因此，`foo` 和 `FOO` 是不同的标识符。

在使用 GNU 扩展时，你也可以使用美元符号 '$'

### 1.2 关键字 { #Keywords }

关键字是特殊的标识符，被保留作为语言本身的一部分。你不能挪为他用。

这里列出 C89 识别的关键字：

```
auto break case char const continue default do double else enum extern
float for goto if int long register return short signed sizeof static
struct switch typedef union unsigned void volatile while
```

ISO C99 增加了以下关键字：

```
inline _Bool _Complex _Imaginary
```

GNU 扩展增加了以下关键字：

```
__FUNCTION__ __PRETTY_FUNCTION__ __alignof __alignof__ __asm
__asm__ __attribute __attribute__ __builtin_offsetof __builtin_va_arg
__complex __complex__ __const __extension__ __func__ __imag __imag__ 
__inline __inline__ __label__ __null __real __real__ 
__restrict __restrict__ __signed __signed__ __thread __typeof
__volatile __volatile__
```
 
C89 和 C99 的 GNU 扩展都可以识别下面这个关键字：

```
restrict
```    
    
### 1.3 常量 { #Constants }

常量就是数字或者字符的字面量，如 `5`、`'m'`。所有常量都有特定的数据类型；你可以使用强制类型转换来明确指定一个常量的类型，或者让编译器根据常量值使用默认类型。

#### 1.3.1 整形常量 { #Integer-Constants }

整形常量是一个数字序列，可以带一个可选的前缀来指示进制。

如果数字序列前带有 `0x` 或 `0X`（zero x 或者 zero X），这个常量就是十六进制的（base 16）。十六进制数的值可以使用数字 `0~9` 还有 字母 `a~f` 以及 `A~F`。例如：

```
0x2f
0x88
0xAB43
0xAbCd
0x1
```

如果第一个数位是 `0`（zero）并且下一个字符不是 `x` 或 `X`，则这个常量就是八进制的（base 8）。八进制的值只能使用数字 `0~7`； 8 和 9 不允许使用。例如：

```
057
012
03
0241
```

不是以上两种的话，一个数字序列就被认为是十进制的（base 10）。十进制的值可以使用数字 `0~9`。例如：

```
459
23901
8
12
```

有不同的整数类型，如短整型（short）、长整型（long）、有符号整型（signed）和无符号整型（unsigned）。你可以在常量后面加时一个或多个字母强制将一个整型常量变成长整型 和/或 无符号整型：

```
u
U
```
无符号整型。

```
l
L
```
长整形。

例如，`45U` 是一个 `unsigned int` 常量。你也可以组合字母：`45UL` 就是一个 `unsigned long int` 常量了（字符可以是任意顺序）。

ISO C99 和 GNU C 扩展都增加了 `long long int` 和 `unsigned long long int` 类型。你可以使用 2 个 L 来得到一个 `long long int` 常量；增加一个 'U' 将得到一个 `unsigned long long int` 常量。例如，`45ULL`。

#### 1.3.2 字符常量 { #Character-Constants }

一个字符常量通常是一个被单引号引起来的单个字符，如 `'Q'`。一个字符常量默认是 `int` 类型。

一些字符，如单引号本身，不能使用一个字符表示。为了表示这样的字符，你可以使用如下的“转义序列”：

`\\` 反斜线

`\?` 问号

`\'` 单引号

`\"` 双引号

`\a` 声音警报

`\b` 退格字符

`\e` `<ESC>` 字符。（这个是 GNU 扩展）

`\f` 馈页

`\n` 换行

`\r` 回车

`\t` 水平制表符

`\v` 垂直制表符

`\o, \oo, \ooo` 八进制数

`\xh, \xhh, \xhhh, …` 十六进制数

使用任何一个转义序列都要使用单引号引起来，并像对待其他字符一样。例如：字母 m 是 'm'；换行符是 '\n'。

八进制数转义序列是一个反斜杠跟着 1 个、2 个或者 3 个八进制数字（0~7）。例如，八进制 101 等于十进制的 65，即 ASCII字符 'A'。因此，字符常量 '\101' 跟字符常量 'A' 是一样的。

十六进制转义序列是一个反斜杠跟着一个 x 以及无限个十六进制数字（0~9, a~f, A~F）。
虽然可能的十六进制数字字符串的长度是无限制的，但是任何给定字符集中的字符常量的数量都不是无限的。（例如，经常使用的扩展的ASCII字符串只有 256 个字符）如果你试图使用一个超过这个字符范围的十六进制值，你会得到一个编译时错误的。

#### 1.3.3 实数常量 { #Real-Number-Constants }

实数常量是一个表示分数（浮点数）的值。它包含一个表示该数整数部分（或者该数）的数字序列，一个小数点，以及一个表示分数部分的数字序列。

整数部分或者分数部分可以缺省，但不能都没有。例如：

```
double a, b, c, d, e, f;

a = 4.7;

b = 4.;

c = 4;

d = .7;

e = 0.7;
```

（在第三个赋值语句中，整数常量 4 被自动从 int 转为 double）

实数常量也可以跟一个 e 或 E，然后是一个整数指数。这个指数可正可负。

```
double x, y;

x = 5e2; /* x is 5 * 100, or 500.0. */
y = 5e-2; /* y is 5 * (1/100), or 0.05. */
```
    
你可以在实数常量尾部加一个字母让它拥有明确的类型。如果你加了一个 F（或者 f）它就是 `float` 类型了；如果你加了 L（或者 l）那它就是 `double` 类型了。如果不加，默认是 `double`。

#### 1.3.4 字符串常量 { #String-Constants }

字符串常量是 0 个或多个字符、数字以及转义字符组成的序列，被双引号引起来。字符串常量算是“字符数组”类型。所有的字符串常量都包含一个 null 终止符（`\0`）作为最后一个字符。字符串作为字符数组来存储，但不包含其大小属性。null 终止符告诉字符串处理函数字符串在哪里结束。

相邻的字符串常量会被拼接（合并）称一个字符串，并在尾部加上 null 终止符。

字符串不能包含双引号，这是因为双引号用来引住字符串。为了能够在字符串中包含双引号，必须使用 `\"` 转义序列。你可以在字符串中使用任何在字符常量中使用的转义序列。下面是几个字符串常量的例子：

```
/* This is a single string constant. */
"tutti frutti ice cream"

/* These string constants will be concatenated, same as above. */
"tutti " "frutti" " ice " "cream"

/* This one uses two escape sequences. */
"\"hello, world!\""
```

如果一个字符串太长导致一行放不下，你可以使用反斜线 `\` 来分割成多行。

```
"Today's special is a pastrami sandwich on rye bread with \
a potato knish and a cherry soda."
```

因为相邻的字符串被自动拼接，所以你可以把长字符串跨越多行写成分离的相邻的多个字符串。例如：

```
"Tomorrow's special is a corned beef sandwich on "
"pumpernickel bread with a kasha knish and seltzer water."
```

和下面这样写是一样的：

```
"Tomorrow's special is a corned beef sandwich on \
pumpernickel bread with a kasha knish and seltzer water."
```

为了插入一个换行符，使得当打印字符串的时候能被打印在 2 行，你要使用转义序列 '\n'。

```
printf ("potato\nknish");
```

会被打印成

```
potato
knish
```

### 1.4 操作符 { #Operators }

操作符是一种特殊的 token，用来对一个、两个或者三个操作数进行运算，如加号（`+`）或者减号（`-`）。操作符的全部内容可以在后面章节找到。详见 [表达式与操作符](/Expressions-And-Operators/)


### 1.5 分隔符 { #Separators }

分隔符用来分割 token。空白符（见下节）是一种分隔符，但它却不是一个 token。其他分隔符本身都是单字符 token：

```
( ) [ ] { } ; , . :
``` 

### 1.6 空白符 { #White-Space }

空白符是用于这几种字符的集体名词：空格符、水平制表符、换行符、垂直制表符以及馈页符。空白符会被忽略掉（在字符串常量和字符常量外面的）因此它们是可有可无，除非他们被用于分割 token。这意味着

```
#include <stdio.h>

int
main()
{
  printf( "hello, world\n" );
  return 0;
}
```

和

```
#include <stdio.h> int main(){printf("hello, world\n");
return 0;}
```

在功能上是相同的程序。

尽管你必须使用空白符来分割许多 token，但操作符和操作数之间不需要空白符，其他分隔符与他们分割的内容之间也不需要空白符。

```
/* 下面这些都是正确的 */

x++;
x ++ ;
x=y+z;
x = y + z ;
x=array[2];
x = array [ 2 ] ;
fraction=numerator / *denominator_ptr;
fraction = numerator / * denominator_ptr ;
```

此外，任何允许使用一个空白符的地方，一定允许使用任意多个空白符。

```
/* These two statements are functionally identical. */
x++;

x
       ++       ;
```

在字符串常量中，空格和水平制表符不能被忽略；确切说，他们是该字符串的一部分。因此

```
"potato knish"
```

和

```
"potato                        knish"
```
 
是不一样的。
