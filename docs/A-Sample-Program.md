为了总结我们对 C 语言的描述，这里用 C 语言写了一个完整的程序，包括一个 C 源文件和一个头文件。这是一个典型的 “hello world” 程序的扩展版本，作为用在 FSF Project GNU 中的代码如何格式和组织结构的样例。（你可以从 [http://www.gnu.org/software/hello](http://www.gnu.org/software/hello) 下载该程序的最新版本，包括样例 makefile 和其他介绍如何编写 GNU 软件的样例）。

这个程序使用了预处理器特性；关于预处理器宏的描述，详见 GCC 文档 [The C Preprocessor](https://gcc.gnu.org/onlinedocs/cpp/)部分。

- [hello.c](#hello-c):
- [system.h](#system-h)


### 7.1 hello.c { #hello-c }

```
/* hello.c -- print a greeting message and exit.

  Copyright (C) 1992, 1995, 1996, 1997, 1998, 1999, 2000, 2001, 2002,
  2005, 2006, 2007 Free Software Foundation, Inc.

  This program is free software; you can redistribute it and/or modify
  it under the terms of the GNU General Public License as published by
  the Free Software Foundation; either version 3, or (at your option)
  any later version.

  This program is distributed in the hope that it will be useful,
  but WITHOUT ANY WARRANTY; without even the implied warranty of
  MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
  GNU General Public License for more details.

  You should have received a copy of the GNU General Public License
  along with this program; if not, write to the Free Software Foundation,
  Inc., 51 Franklin Street, Fifth Floor, Boston, MA 02110-1301, USA.  */

#include <config.h>
#include "system.h"

/* String containing name the program is called with.  */
const char *program_name;

static const struct option longopts[] =
{
    { "greeting", required_argument, NULL, 'g' },
    { "help", no_argument, NULL, 'h' },
    { "next-generation", no_argument, NULL, 'n' },
    { "traditional", no_argument, NULL, 't' },
    { "version", no_argument, NULL, 'v' },
    { NULL, 0, NULL, 0 }
};

static void print_help (void);
static void print_version (void);

int
main (int argc, char *argv[])
{
    int optc;
    int t = 0, n = 0, lose = 0;
    const char *greeting = NULL;

    program_name = argv[0];

    /* Set locale via LC_ALL.  */
    setlocale (LC_ALL, "");

#if ENABLE_NLS
    /* Set the text message domain.  */
    bindtextdomain (PACKAGE, LOCALEDIR);
    textdomain (PACKAGE);
#endif

    /* Even exiting has subtleties.  The /dev/full device on GNU/Linux
       can be used for testing whether writes are checked properly.  For
       instance, hello >/dev/full should exit unsuccessfully.  On exit,
       if any writes failed, change the exit status.  This is
       implemented in the Gnulib module "closeout".  */

    atexit (close_stdout);

    while ((optc = getopt_long (argc, argv, "g:hntv", longopts, NULL)) != -1)
        switch (optc)
        {
        /* One goal here is having --help and --version exit immediately,
         per GNU coding standards.  */
        case 'v':
            print_version ();
            exit (EXIT_SUCCESS);
            break;
        case 'g':
            greeting = optarg;
            break;
        case 'h':
            print_help ();
            exit (EXIT_SUCCESS);
            break;
        case 'n':
            n = 1;
            break;
        case 't':
            t = 1;
            break;
        default:
            lose = 1;
            break;
        }

    if (lose || optind < argc)
    {
        /* Print error message and exit.  */
        if (optind < argc)
            fprintf (stderr, _("%s: extra operand: %s\n"),
                     program_name, argv[optind]);
        fprintf (stderr, _("Try `%s --help' for more information.\n"),
                 program_name);
        exit (EXIT_FAILURE);
    }

    /* Print greeting message and exit. */
    if (t)
        printf (_("hello, world\n"));

    else if (n)
        /* TRANSLATORS: Use box drawing characters or other fancy stuff
           if your encoding (e.g., UTF-8) allows it.  If done so add the
           following note, please:

           [Note: For best viewing results use a UTF-8 locale, please.]
        */
        printf (_("\
+---------------+\n\
| Hello, world! |\n\
+---------------+\n\
"));

    else
    {
        if (!greeting)
            greeting = _("Hello, world!");
        puts (greeting);
    }

    exit (EXIT_SUCCESS);
}




/* Print help info.  This long message is split into
   several pieces to help translators be able to align different
   blocks and identify the various pieces.  */

static void
print_help (void)
{
    /* TRANSLATORS: --help output 1 (synopsis)
       no-wrap */
    printf (_("\
Usage: %s [OPTION]...\n"), program_name);

    /* TRANSLATORS: --help output 2 (brief description)
       no-wrap */
    fputs (_("\
Print a friendly, customizable greeting.\n"), stdout);

    puts ("");
    /* TRANSLATORS: --help output 3: options 1/2
       no-wrap */
    fputs (_("\
  -h, --help          display this help and exit\n\
  -v, --version       display version information and exit\n"), stdout);

    puts ("");
    /* TRANSLATORS: --help output 4: options 2/2
       no-wrap */
    fputs (_("\
  -t, --traditional       use traditional greeting format\n\
  -n, --next-generation   use next-generation greeting format\n\
  -g, --greeting=TEXT     use TEXT as the greeting message\n"), stdout);

    printf ("\n");
    /* TRANSLATORS: --help output 5 (end)
       TRANSLATORS: the placeholder indicates the bug-reporting address
       for this application.  Please add _another line_ with the
       address for translation bugs.
       no-wrap */
    printf (_("\
Report bugs to <%s>.\n"), PACKAGE_BUGREPORT);
}




/* Print version and copyright information.  */

static void
print_version (void)
{
    printf ("hello (GNU %s) %s\n", PACKAGE, VERSION);
    /* xgettext: no-wrap */
    puts ("");

    /* It is important to separate the year from the rest of the message,
       as done here, to avoid having to retranslate the message when a new
       year comes around.  */
    printf (_("\
Copyright (C) %s Free Software Foundation, Inc.\n\
License GPLv3+: GNU GPL version 3 or later\
<http://gnu.org/licenses/gpl.html>\n\
This is free software: you are free to change and redistribute it.\n\
There is NO WARRANTY, to the extent permitted by law.\n"),
            "2007");
}
```


### 7.2 system.h { #system-h }

```
 /* system.h: system-dependent declarations; include this first.
   Copyright (C) 1996, 2005, 2006, 2007 Free Software Foundation, Inc.

   This program is free software; you can redistribute it and/or modify
   it under the terms of the GNU General Public License as published by
   the Free Software Foundation; either version 3, or (at your option)
   any later version.

   This program is distributed in the hope that it will be useful,
   but WITHOUT ANY WARRANTY; without even the implied warranty of
   MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
   GNU General Public License for more details.

   You should have received a copy of the GNU General Public License
   along with this program; if not, write to the Free Software Foundation,
   Inc., 51 Franklin Street, Fifth Floor, Boston, MA 02110-1301, USA.  */

#ifndef HELLO_SYSTEM_H
#define HELLO_SYSTEM_H

/* Assume ANSI C89 headers are available.  */
#include <locale.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

/* Use POSIX headers.  If they are not available, we use the substitute
   provided by gnulib.  */
#include <getopt.h>
#include <unistd.h>

/* Internationalization.  */
#include "gettext.h"
#define _(str) gettext (str)
#define N_(str) gettext_noop (str)

/* Check for errors on write.  */
#include "closeout.h"

#endif /* HELLO_SYSTEM_H */
```
