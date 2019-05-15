这是一份由 GNU Compiler Collection（GCC）实现的 C 编程语言的参考手册。特别指出，这份手册针对的是：

- 1989 ANSI C 标准，俗称 “C89”
- 1999 ISO C 标准，俗称 “C99”，针对 GCC 实现的 C99 部分
- 当前的 GNU 标准 C 扩展

这份手册以 C89 作为基础。C99 特性以及 GNU 扩展都会明确标注。

默认情况下，GCC 会把代码当做 C89+GNU 特定扩展来编译。大部分 C99 特性已经被支持；一旦所有特性可用，默认编译语言将改为 C99+GNU 特定扩展（一些针对 C89 的 GNU 扩展将被移除，有的做了轻微改动以作为 C99 中的标准特性）。

C 语言包含了一系列预处理指令，被用于宏文本替换，条件编译以及文件引入。尽管通常会在 C 语言手册中描述，但 GNU C 预处理器被完全归档在了 *C 预处理器* 中。该文档覆盖了 C、C++ 以及 Objective-C 的预处理，因此这里将不再包含这些内容。

### 致谢 { #Credits }

非常感谢帮助编辑、校对、提供建议、排版以及文案的所有人，包括：Diego Andres Alvarez Marin, Nelson H. F. Beebe, Karl Berry, Robert Chassell, Hanfeng Chen, Mark de Volld, Antonio Diaz Diaz, dine, Andreas Foerster, Denver Gingerich, Lisa Goldstein, Robert Hansen, Jean-Christophe Helary, Mogens Hetsholm, Teddy Hogeborn, Joe Humphries, J. Wren Hunt, Dutch Ingraham, Adam Johansen, Vladimir Kadlec, Benjamin Kagia, Dright Kayorent, Sugun Kedambadi, Felix Lee, Bjorn Liencres, Steve Morningthunder, Aljosha Papsch, Matthew Plant, Jonathan Sisti, Richard Stallman, J. Otto Tennant, Ole Tetlie, Keith Thompson, T.F. Torrey, James Youngman, and Steve Zachar。Trevis Rothwell 作为项目维护人员 和 James Youngman 一起写了大部分的内容。

一些示例程序是基于 Donald Knuth 的 *计算机程序设计艺术* 中的算法。

bug 报告和建议请发邮件至 [gnu-c-manual@gnu.org](mailto:gnu-c-manual@gnu.org)

