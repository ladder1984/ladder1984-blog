---
title: Visual Studio 2013支持的C99库（library）
tags:
  - C
id: 1141
categories:
  - Coding
date: 2014-07-23 18:09:05
---

# 

C99已经发布多年，但微软的Visual C++尚未完整支持C99。我偶然看到VS官方博客上一篇介绍C99库支持的文章，特翻译了一下，希望对大家有帮助。水平有限，翻译不当之处，欢迎指正。

* * *

大家好，我是Pat Brenner，Visual C++库团队的开发人员。在这篇博文中，我想分享添加到Visual Studio 2013中的 C运行时库（run-time library）对C99支持的一些信息。

总的来说，我们为如下头文件中缺失的函数增加了声明（declarations）和实现（implementations）：math.h，ctype.h，wctype.h，tdio.h， stdlib.h， and wchar.h。我们也新增了一些头文件，包括complex.h，stdbool.h，fenv.h，和 inttypes.h，并且增加了声明在它们中的所有函数的实现。此外，我们增加了新的C ++ wrapper headers（ccomplex, cfenv, cinttypes, ctgmath)并且更新了其他的一部分（ccomplex, cctype, clocale, cmath, cstdint, cstdio, cstring, cwchar, and cwctype）。

大部分工作（除了stdbool.h和fenv.h外的所有C头文件）已经及时在Visual Studio 2013 Preview发布前完成并且已经可用，但是剩下的（stdbool.h, fenv.h 和 the C++ wrapper headers）将会在Visual Studio 2013 RTM中完成并发布。

更详细的说，这些是我们增加的声明和实现，根据声明它们的头文件分组：

*   math.h:

    *   float_t, double_t, fpclassify, isfinite isinf, isnan, isnormal, signbit
    *   HUGE_VALF, HUGE_VALL, INFINITY, NAN, MATH_ERRNO, MATH_ERREXCEPT
    *   FP_INFINITE, FP_NAN, FP_NORMAL, FP_SUBNORMAL, FP_ZERO, FP_ILOGB0, FP_ILOGBNAN
    *   acosh, acoshf, acoshl, asinh, asinhf, asinhl, atanh, atanhf, atanhl
    *   exp2, exp2f, exp2l, expm1, expm1f, expm1l
    *   ilogb, ilogbf, ilogbl, logb, logbf, logbl, log1p, log1pf, log1pl, log2, log2f, log2l
    *   scalbn, scalbnf, scalbnl, scalbln, scalblnf, scalblnl
    *   cbrt, cbrtf, cbrtl, erf, erff, erfl, erfc, erfcf, erfcl
    *   lgamma, lgammaf, lgammal, tgamma, tgammaf, tgammal
    *   nearbyint, nearbyintf, nearbyintl, nan, nanf, nanl
    *   rint, rintf, rintl, lrint, lrintf, lrintl, llrint, llrintf, llrintl
    *   round, roundf, roundl, lround, lroundf, lroundl, llround, llroundf, llroundl
    *   trunc, truncf, truncl, remainder, remainderf, remainder, remquo, remquof, remquol
    *   nextafter, nextafterf, nextafterl, nexttoward, nexttowardf, nexttowardl
    *   fdim, fdimf, fdiml, fmax, fmaxf, fmaxl, fmin, fminf, fminl, fma, fmaf, fmal

*   complex.h:

    *   cacos, cacosf, cacosl, casin, casinf, casinl, catan, catanf, catanl
    *   ccos, ccosf, ccosl, csin, csinf, csinl, ctan, ctanf, ctanl
    *   cacosh, cacoshf, cacoshl, casinh, casinhf, casinhl, catanh, catanhf, catanhl
    *   ccosh, ccoshf, ccoshl, csinh, csinhf, csinhl, ctanh, ctanhf, ctanhl
    *   cexp, cexpf, cexpl, clog, clogf, clogl, cabs, cabsf, cabsl
    *   cpow, cpowf, cpowl, csqrt, csqrtf, csqrtl, carg, cargf, cargl
    *   cimag, cimagf, cimagl, conj, conjf, conjl, cproj, cprojf, cprojl, creal, crealf, creall

*   fenv.h:

    *   fegetenv, fesetenv, feupdateenv, fegetexceptflag, fesetexceptflag
    *   feclearexcept, feholdexcept, fetestexcept, feraiseexcept

*   inttypes.h:

    *   PRIi8, PRIi16, PRIi32, PRIi64, PRIiMAX, PRIiPTR, PRIiLEAST8, PRIiLEAST16, PRIiLEAST32, PRIiLEAST64, PRIiFAST8, PRIiFAST16, PRIiFAST32, PRIiFAST64
    *   PRIo8, PRIo16, PRIo32, PRIo64, PRIoMAX, PRIoPTR, PRIoLEAST8, PRIoLEAST16, PRIoLEAST32, PRIoLEAST64, PRIoFAST8, PRIoFAST16, PRIoFAST32, PRIoFAST64
    *   PRIu8, PRIu16, PRIu32, PRIu64, PRIuMAX, PRIuPTR, PRIuLEAST8, PRIuLEAST16, PRIuLEAST32, PRIuLEAST64, PRIuFAST8, PRIuFAST16, PRIuFAST32, PRIuFAST64
    *   PRIx8, PRIx16, PRIx32, PRIx64, PRIxMAX, PRIxPTR, PRIxLEAST8, PRIxLEAST16, PRIxLEAST32, PRIxLEAST64, PRIxFAST8, PRIxFAST16, PRIxFAST32, PRIxFAST64
    *   PRIX8, PRIX16, PRIX32, PRIX64, PRIXMAX, PRIXPTR, PRIXLEAST8, PRIXLEAST16, PRIXLEAST32, PRIXLEAST64, PRIXFAST8, PRIXFAST16, PRIXFAST32, PRIXFAST64
    *   SCNd8, SCNd16, SCNd32, SCNd64, SCNdMAX, SCNdPTR, SCNdLEAST8, SCNdLEAST16, SCNdLEAST32, SCNdLEAST64, SCNdFAST8, SCNdFAST16, SCNdFAST32, SCNdFAST64
    *   SCNi8, SCNi16, SCNi32, SCNi64, SCNiMAX, SCNiPTR, SCNiLEAST8, SCNiLEAST16, SCNiLEAST32, SCNiLEAST64, SCNiFAST8, SCNiFAST16, SCNiFAST32, SCNiFAST64
    *   SCNo8, SCNo16, SCNo32, SCNo64, SCNoMAX, SCNoPTR, SCNoLEAST8, SCNoLEAST16, SCNoLEAST32, SCNoLEAST64, SCNoFAST8, SCNoFAST16, SCNoFAST32, SCNoFAST64
    *   SCNu8, SCNu16, SCNu32, SCNu64, SCNuMAX, SCNuPTR, SCNuLEAST8, SCNuLEAST16, SCNuLEAST32, SCNuLEAST64, SCNuFAST8, SCNuFAST16, SCNuFAST32, SCNuFAST64
    *   SCNx8, SCNx16, SCNx32, SCNx64, SCNxMAX, SCNxPTR, SCNxLEAST8, SCNxLEAST16, SCNxLEAST32, SCNxLEAST64, SCNxFAST8, SCNxFAST16, SCNxFAST32, SCNxFAST64
    *   SCNX8, SCNX16, SCNX32, SCNX64, SCNXMAX, SCNXPTR, SCNXLEAST8, SCNXLEAST16, SCNXLEAST32, SCNXLEAST64, SCNXFAST8, SCNXFAST16, SCNXFAST32, SCNXFAST64
    *   imaxabs, imaxdiv, strtoimax, strtoumax, wcstoimax, wcstoumax

*   ctype.h

    *   isblank

*   wctype.h

    *   iswblank

*   float.h

    *   DECIMAL_DIG, FLT_EVAL_METHOD

*   stdarg.h

    *   va_copy

*   stdbool.h

    *   bool, true, false, __bool_true_false_are_defined

*   stdio.h

    *   vscanf, vfscanf, vsscanf

*   stdlib.h

    *   atoll, strtof, strtold, strtoll, strtoull

*   wchar.h

    *   vwscanf, vfwscanf, vswscanf, wcstof, wcstold, wcstoll, wcstoull
我们知道这并不是对C99库函数的完整支持，尽我们的理解，缺少的部分如下：

*   tgmath头文件缺失，这个头文件需要C编译器的支持。

    *   注意，ctgmath 头文件已经被添加——这是有可能的，因为这个头文件不需要tgmath.h头文件，只需要ccomplex 和 cmath headers头文件

*   uchar.h偷文件缺失。这来自the C Unicode TR.
*   printf家族中的一些格式说明符尚不支持
*   stdio.h和wcahr.h中的snprintf and snwprintf 函数缺失。
我希望你觉得这些信息有用，我们尽力优先实现我们认为重要的函数。

Pat Brenner, Visual C++ Libraries Development Team

* * *

&nbsp;

原文链接：[http://blogs.msdn.com/b/vcblog/archive/2013/07/19/c99-library-support-in-visual-studio-2013.aspx](http://blogs.msdn.com/b/vcblog/archive/2013/07/19/c99-library-support-in-visual-studio-2013.aspx)

&nbsp;