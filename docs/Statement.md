语句用来执行动作以及控制程序流。你也可以写不做任何事情的语句，或做一些丝毫没有用的事情。

### 4.1 标签 { #Labels }

你可以使用标签来标识源代码的一节，然后可以在后面的 `goto` 中使用（详见 [goto 语句](#The-goto-Statement)）。一个标签包含一个标识符（就像之前用于变量名的一样）后跟一个冒号。如：

```
treet:
```

但你要明白标签名字和其他标识符名字是不冲突的：

```
int treet = 5;    /* treet the variable. */
treet:            /* treet the label. */
```

ISO C 标准要求标签后面必须至少跟一个语句，即便是一个空语句（详见 [空语句](#The-Null-Statement)）。GCC 并不要求，但是要注意，如果你经常这样用，你的代码会有兼容性问题。

### 4.2 表达式语句 { #Expression-Statements }

你可以在任何表达式后面加上分号，来把它变成一个语句。例如：

```
5;
2 + 2;
10 >= 9;
```

在上面的各个语句中，表达式都会被求值。然而这并没什么卵用，因为他们既没有把值存起来，也没有做任何实际的事情，仅仅是计算了它们本身而已。编译器会忽略掉这种语句。

表达式语句仅在有副作用时有用，如存储一个值、函数调用或造成一个错误（这可能有点难懂）。例如：

```
x++;
y = x + 25;
puts ("Hello, user!");
*cucumber;
```

最后的语句 `*cucumber;` 可能会造成一个错误，如果 `cucumber` 的值不是一个合法的指针或者被声明为了 `volatile`。

### 4.3 if 语句 { #The-if-Statement }

你可以使用 `if` 语句来有条件地执行你的部分程序，执行部分取决于你给出的表达式的真值。常用形式如下：

```
if (test)
    then-statement
else
    else-statement
```

如果 *test* 求值为真，那么执行 *then-statement* 语句而 *else-statement* 不会被执行。相反如果 *test* 为假，则 *else-statement* 被执行而 *then-statement* 不会被执行。`else` 从句是可选的。例如：

```
if (x == 10)
    puts ("x is 10");
```

如果 `x == 10` 是真，语句 `puts ("x is 10");` 被执行。如果 `x == 10` 是假，语句 `puts ("x is 10");` 不会被执行。

下面是使用 `else` 的例子：

```
if (x == 10)
    puts ("x is 10");
else
    puts ("x is not 10");
```

你也可以使用一系列 `if` 语句来测试多个条件：

``` 
if (x == 1)
    puts ("x is 1");
else if (x == 2)
    puts ("x is 2");
else if (x == 3)
    puts ("x is 3");
else
    puts ("x is something else");
```

下面的代码根据给定的年 `y` 来计算和打印复活节的日期：

```
void
easterDate (int y)
{
    int n = 0;
    int g = (y % 19) + 1;
    int c = (y / 100) + 1;
    int x = ((3 * c) / 4) - 12;
    int z = (((8 * c) + 5) / 25) - 5;
    int d = ((5 * y) / 4) - x - 10;
    int e = ((11 * g) + 20 + z - x) % 30;
    
    if (((e == 25) && (g > 11)) || (e == 24))
        e++;
    
    n = 44 - e;
    
    if (n < 21)
        n += 30;
    
    n = n + 7 - ((d + n) % 7);
    
    if (n > 31)
        printf ("Easter: %d April %d", n - 31, y);
    else
        printf ("Easter: %d March %d", n, y);
}
```

### 4.4 switch 语句 { #The-switch-Statement }

你可以使用 `switch` 语句来把一个表达式和其他表达式作比较，然后根据比较结果决定是否执行一系列子语句。下面是 `switch` 的一般形式：

```
switch (test)
{
case compare-1:
    if-equal-statement-1
case compare-2:
    if-equal-statement-2
…
default:
    default-statement
}
```

`switch` 语句比较 *test* 和每一个 *compare* 表达式，直到找到一个和 *test* 相等的。然后该分支下面的语句会被执行。所有被比较的表达式必须是整数类型，并且 *compare-N* 必须是一个常量整型（例如，一个字面量整数或一个字面量整数组成的表达式）。

可选地，你可以指定一个 默认（default） 分支。如果上面的分支都没有匹配到就会执行该默认分支。一般 default 都会放在最后，但这不是强制的。

```
switch (x)
{
case 0:
    puts ("x is 0");
    break;
case 1:
    puts ("x is 1");
    break;
default:
    puts ("x is something else");
    break;
}
```

注意到，上面每个分支都用了一个 `break` 语句。这是因为，如果一个分支被匹配到，不只是它的子句被执行，所有它下面的分支的子句也会执行：

```
int x = 0;
switch (x)
{
case 0:
    puts ("x is 0");
case 1:
    puts ("x is 1");
default:
    puts ("x is something else");
}
```

这个例子的输出是：

```
x is 0
x is 1
x is something else
```

这通常不是我们想要的。在每一个分支后面加一个 `break` 会将程序的执行流程引导到 `switch` 语句之后。

作为 GNU C 扩展，你可以在一个 `case` 标签里指定一个连续的整数值范围，如：

```
case low ... high:
```

这个把该范围对应的各个数字 -- 包含 *low* 和 *high* -- 写在各自标签里的效果是一样的。

这个特性对于一个范围的 ASCII 字符尤其有用：

```
case 'A' ... 'Z':
```

要注意，在 ... 两边都有空格；否则会解析出错。例如，可以这样写：

```
case 1 ... 5:
```

而不是

```
case 1...5:
```

通常使用 `switch` 语句来处理多种可能的 `errno`。这种情况下，一个可移植的程序应该注意到有可能 2 个使用 `errno` 的宏实际却有相同的值，例如 `EWOULDBLOCK` 和 `EAGAIN`。

### 4.5 while 语句 { #The-while-Statement }

`while` 语句是一种在循环开始前进行退出测试的循环语句。下面是其一般形式：

```
while (test)
    statement
```

`while` 语句先对 *test* 求值。如果 *test* 是真，则 s*tatement* 被执行，然后再次对 *test* 求值。只要 *test* 为真，*statement* 会继续重复执行。

下面的例子打印整数 0 到 9：

```
int counter = 0;
while (counter < 10)
    printf ("%d ", counter++);
```

`break` 语句可以终止 `while` 循环。

### 4.6 do 语句 { #The-do-Statement }

`do` 语句是在循环末尾进行退出测试的循环语句。下面是其一般形式：

```
do
    statement
while (test);
```

`do` 语句先执行 *statement*。之后对 *test* 求值。如果 *test* 为真，*statement* 再次被执行。只要 *test* 为真，*statement* 会继续重复执行。

下面的例子打印整数 0 到 9：

```
int x = 0;
do
    printf ("%d ", x++);
while (x < 10);
```

`break` 语句可以终止 `do` 循环。

### 4.7 for 语句 { #The-for-Statement }

`for` 是一种循环语句，它的结构可以简单地变量初始化，表达式测试以及变量修改。方便于进行计数控制的循环。它的一般形式如下：

```
for (initialize; test; step)
    statement
```

`for` 循环先计算表达式 *initialize*，然后对表达式 *test* 求值。如果 *test* 为假，循环结束并且程序的控制流程恢复到 `for` 语句之后；否则，*statement* 被执行。最后，计算 *step*。下一次循环迭代会先再次对 *test* 求值。

通常，*initialize* 会给一个或多个变量赋值，一般是计数器； *test* 会把这些变量和预定义的表达式比较；而 *step* 用来修改这些变量。下面也是一个打印整数 0 ~ 9 的例子：

```
int x;
for (x = 0; x < 10; x++)
    printf ("%d ", x);
```

首先对 *initialize* 求值，也就是把 0 赋给 `x`。然后只要 `x` 小于 10 就会把它打印出来（在循环体中）。接着 `x` 在 *step* 子句里被递增，然后 *test* 再被求值。

这 3 个表达式在 `for` 语句中都是可选的，其中可以任意组合。既然第一个表达式只被求值一次，所以经常会被省略。上面的例子可以这样写：

```
int x = 1;
for (; x <= 10; x++)
    printf ("%d ", x);
```

在这个例子中，`x` 在 `for` 语句之前就被赋值了。

如果你不写 *test* 表达式，那 `for` 语句就变成无限循环了（除非你在 `for` 语句中使用了 `break` 或者 `goto` 语句）。这和 *test* 为 1 是一样的，永远为真。

下面的例子从 1 开始打印所有的正整数：

```
for (x = 1; ; x++)
    printf ("%d ", x);
```

如果你不写 *step* 表达式，就没有了完成循环的步进 -- 至少不像 `for` 语句正常期望的那样。

下面的例子会不停地打印 1：

```
for (x = 1; x <= 10;)
    printf ("%d ", x);
```

可能会引起困惑的是，你不能使用逗号运算符（详见 [逗号操作符](Expressions-And-Operators.md#The-Comma-Operator)）在同一个 `for` 语句中监控多个变量，因为通常逗号运算符会丢弃它的左操作数的结果。如：

```
int x, y;
for (x = 1, y = 10; x <= 10, y >= 1; x+=2, y--)
    printf ("%d %d\n", x, y);
```

输出是：

```
1 10
3 9
5 8
7 7
9 6
11 5
13 4
15 3
17 2
19 1
```

如果你要测试 2 个条件，你需要用 `&&` 运算符：

```
int x, y;
for (x = 1, y = 10; x <= 10 && y >= 1; x+=2, y--)
    printf ("%d %d\n", x, y);
```

`break` 语句也可以终止 `for` 循环：

下面的例子是一个计算平方和的函数，给定一个起始数，一个终止数，来计算中间所有数的平方和：

```
int
sum_of_squares (int start, int end)
{
    int i, sum = 0;
    for (i = start; i <= end; i++)
    sum += i * i;
    return sum;
}
```

### 4.8 代码块 { #Blocks }

一个代码块是 0 个或多个用花括号包起来的语句，也叫做组合语句。通常代码块用在条件语句和循环语句的主体部分，用来将一组语句放在一起。

```
for (x = 1; x <= 10; x++)
{
    printf ("x is %d\n", x);
        
    if ((x % 2) == 0)
        printf ("%d is even\n", x);
    else
        printf ("%d is odd\n", x);
}
```

代码块可以嵌套：

```
for (x = 1; x <= 10; x++)
{
    if ((x % 2) == 0)
    {
        printf ("x is %d\n", x);
        printf ("%d is even\n", x);
    }
    else
    {
        printf ("x is %d\n", x);
        printf ("%d is odd\n", x);
    }
}
```

你可以在代码块内声明变量；这样的变量是该块的局部变量。在 C89 标准中，声明语句必须放在其他语句前，所以有时引入语句块来定义变量就很有用：

```
{
    int x = 5;
    printf ("%d\n", x);
}
printf ("%d\n", x);   /* Compilation error! x exists only
                       in the preceding block. */
```

### 4.9 空语句 { #The-Null-Statement }

空语句只有一个分号。

```
;
```

空语句不做任何事情。不在任何地方存值。程序执行时不会耗时。

通常，空语句会用做循环语句的循环体，或者作为 `for` 语句中的一个或多个表达式。下面是一个空语句作为 `for` 循环的循环体的例子（顺便计算一下 n 的整数平方根，供君一笑）：

```
for (i = 1; i*i < n; i++)
    ;
```

下面是另一个用作 `for` 循环体的例子，但是会产生输出：

```
for (x = 1; x <= 5; printf ("x is now %d\n", x), x++)
    ;
```

空语句有时也会跟在一个标签后面，用作该语句块的最后一个语句。

### 4.10 goto 语句 { #The-goto-Statement }

你可以使用 `goto` 语句在程序中无条件跳转。形式一般为：

```
goto label;
```

你必须指定目的标签；在 `goto` 语句执行时，程序就跳到了该标签处。详见 [标签](#Labels) 一节。这里有个例子：

```
goto end_of_program;
…
end_of_program:
```

标签可以在同一函数的任何地方，但 `goto` 跳不到其它函数。

你*可以*用 `goto` 来模拟循环，但我们不推荐这样做 -- 它让程序难以理解，并且 GCC 无法很好地优化它。你应该尽可能使用 `for`、`while` 以及 `do`
语句来实现循环。

GCC 扩展允许 `goto` 语句跳到 `void*` 指向的地址。要这样做，你还需要使用一元运算符 `&&`（不是 `&`）拿到标签的地址。下面是一个瞎造的例子：

```
enum Play { ROCK=0, PAPER=1, SCISSORS=2 };
enum Result { WIN, LOSE, DRAW };

static enum Result turn (void) 
{
  const void * const jumptable[] = {&&rock, &&paper, &&scissors};
  enum Play opp;                /* opponent’s play */
  goto *jumptable[select_option (&opp)];
 rock:
  return opp == ROCK ? DRAW : (opp == PAPER ? LOSE : WIN);
 paper:
  return opp == ROCK ? WIN  : (opp == PAPER ? DRAW : LOSE);
 scissors:
  return opp == ROCK ? LOSE : (opp == PAPER ? WIN  : DRAW);
}
```

### 4.11 break 语句 { #The-break-Statement }

你可以使用 `break` 语句来终止 `while`、`do`、`for` 和 `switch` 语句。例如：

```
int x;
for (x = 1; x <= 10; x++)
{
    if (x == 8)
        break;
    else
        printf ("%d ", x);
}
```

这个例子会打印 1 到 7。当 `x` 自增到 8 时， `x == 8` 为真，因此 `break` 语句被执行，`for` 循环也就被终止了。

如果你把 `break` 放在了嵌套循环或 `switch` 的内层循环或内层 `switch` 中，那 `break` 只会终止最内层的循环或 `switch`。

### 4.12 continue 语句 { #The-continue-Statement }

你可以在循环中使用 `continue` 语句来终止循环的当前一次迭代，然后开始下一次迭代。例如：

```
for (x = 0; x < 100; x++)
{
    if (x % 2 == 0)
        continue;
    else
        sum_of_odd_numbers + = x;
}
```

如果你把 `continue` 放在了嵌套循环的内层循环中，那 `continue` 只会终止最内层的循环。

### 4.13 return 语句 { #The-return-Statement }

你可以使用 `return` 语句来结束一个函数的执行，并将程序的控制交还到该函数的调用处。下面是 `return` 语句的一般形式：

```
return return-value;
```

*return-value* 是可选的。如果函数的返回值类型是 `void`，那返回一个表达式就是非法的；然而，你可以使用不带返回值的 `return` 语句。

如果函数的返回值类型和 *return-value* 的类型不一致，并且无法进行自动类型转换，那返回该 *return-value* 是非法的。

如果函数的返回值类型不是 `void`，并且没有返回值被指定，那该 `return` 语句是合法的，除非在函数被调用的上下文需要一个返回值：

```
x = cosine (y);
```

在这个例子中，函数 `cosine` 被调用处需要一个返回值赋给 `x`。

即使上下文不需要一个返回值，缺省返回值为非 `void` 的函数的返回值不是个好习惯。GCC 中，你可以使用命令行参数 `-Wreturn-type`，在这样的函数中缺省返回值会发出警告。

下面的例子在 `void` 和非 `void` 函数中使用 `return` 语句：

```
void
print_plus_five (int x)
{
    printf ("%d ", x + 5);
    return;
}
int
square_value (int x)
{
    return x * x;
}
```

### 4.14 typedef 语句 { #The-typedef-Statement }

你可以使用 `typedef` 语句来给数据类型创建别名。下面是其一般形式：

```
typedef old-type-name new-type-name
```

*old-type-name* 是该类型已经存在的名字，可能由不只一个标记组成（如 `unsigned long int`）。*new-type-name* 是该类型的新名字，并且必须是一个标识符。创建新名字后旧名字同样可用。例如：

```
typedef unsigned char byte_type;
typedef double real_number_type;
```

对于自定义数据类型，你可以在定义该类型时就给它起一个新名字：

```
typedef struct fish
{
    float weight;
    float length;
    float probability_of_being_caught;
} fish_type;
```

要给一个数组定义类型，你得先给出其元素的类型，然后是新的类型名，末尾接上元素个数：

```
typedef char array_of_bytes [5];
array_of_bytes five_bytes = {0, 1, 2, 3, 4};
```

在选择类型的名字时，你要避免使用 `_t` 后缀。虽然编译器允许这么做，但 POSIX 标准将其保留作为标准库里的类型名字。
