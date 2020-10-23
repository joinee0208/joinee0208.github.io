---
layout: part
title: C语言版本
original: true
typora-root-url: ..\
---

C语言主要有以下几个版本：

- C89/C90(又称ANSI C)：1989年批准通过，1990年发布。
- C94/C95：1995年，针对之前1900年发布的标准，发布的一个修订版。
- C99：1999年发布的新标准。
- C11：2011年发布的新标准。

关于GCC中对于C的扩展：

对于GCC来说，对于上述各个版本的C语言，都有一定的扩展（extension）

如果用-std=C89/C99/C11参数的话，则不使用这些扩展。

如果想要在GCC中使用C扩展的话，所用的参数都是gnuXX类型的，分别是：

- C90使用GCC的C扩展：-std=gnu90
- C99使用GCC的C扩展：-std=gnu99
- C11使用GCC的C扩展：-std=gnu11

当前C language dialect默认所用的参数是：-std=gnu90

但是当以后，GCC对于C99和C11支持程度真正完善后，则可能会换成对应的：-std=gnu99或-std=gnu11。



另外在NXP的GCC工具中，我们默认是GNU90的标准：



By default, GCC provides some extensions to the C language that on rare occasions conflict

with the C standard. See Chapter 6 [Extensions to the C Language Family], page 353.

Use of the ‘-std’ options listed above will disable these extensions where they conflict with

the C standard version selected. You may also select an extended version of the C language

explicitly with ‘-std=gnu90’ (for C90 with GNU extensions), ‘-std=gnu99’ (for C99 with

GNU extensions) or ‘-std=gnu11’ (for C11 with GNU extensions). **The default, if no C language**

**dialect options are given, is ‘-std=gnu90’**;

