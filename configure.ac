#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

AC_PREREQ([2.63])
AC_INIT([libpll], [0.2.0], [Tomas.Flouri@h-its.org])
AM_INIT_AUTOMAKE([subdir-objects])
AC_LANG([C])
AC_CONFIG_SRCDIR([src/pll.c])
AC_CONFIG_HEADERS([config.h])
AC_CONFIG_MACRO_DIR([m4])
AC_CANONICAL_HOST

# set default gcc options
CFLAGS='-g'

# Checks for programs.
AC_PROG_CC
AC_PROG_LIBTOOL
AC_PROG_LEX
AC_PROG_YACC
AC_PROG_INSTALL

if test "x$LEX" != xflex; then
  AC_MSG_ERROR(could not find required installation of FLEX)
fi

if test "x$YACC" != x"bison -y"; then
  AC_MSG_ERROR(could not find required installation of BISON)
fi

LT_INIT

# Checks for libraries.
AC_CHECK_LIB([m],[exp])

# Checks for header files.
AC_CHECK_HEADERS([assert.h math.h stdio.h stdlib.h string.h ctype.h x86intrin.h])

# Checks for typedefs, structures, and compiler characteristics.
AC_C_INLINE
AC_TYPE_SIZE_T
AC_TYPE_UINT32_T
AC_TYPE_INT64_T
AC_TYPE_UINT64_T
AC_TYPE_UINT8_T

# Checks for library functions.
AC_FUNC_MALLOC
AC_FUNC_ALLOCA
AC_FUNC_REALLOC
AC_CHECK_FUNCS([asprintf memcpy memset posix_memalign])

have_avx=no
have_sse=no
have_ps2pdf=no

AX_EXT

if test "x${ax_cv_have_avx_ext}" = "xyes"; then
  have_avx=yes
fi

if test "x${ax_cv_have_sse3_ext}" = "xyes"; then
  have_sse=yes
fi

AC_ARG_ENABLE(avx, AS_HELP_STRING([--disable-avx], [Build without AVX support]))
AS_IF([test "x${ax_cv_have_avx_ext}" = "xyes"], [
  have_avx=yes
])

AC_ARG_ENABLE(sse,i AS_HELP_STRING([--disable-sse],[Build without SSE support]))
AS_IF([test "x${ax_cv_have_sse3_ext}" = "xyes"], [
  have_sse=yes
])

AC_ARG_ENABLE(pdfman, AS_HELP_STRING([--disable-pdfman], [Disable PDF manual creation]))
AS_IF([test "x$enable_pdfman" != "xno"], [
  have_ps2pdf=yes
  AC_CHECK_PROG(HAVE_PS2PDF, ps2pdf, yes, no)
  if test "x$HAVE_PS2PDF" = "xno"; then
    AC_MSG_WARN([*** ps2pdf is required to build a PDF version of the manual])
    have_ps2pdf=no
  fi
])

AM_CONDITIONAL(HAVE_AVX, test "x${have_avx}" = "xyes")
AM_CONDITIONAL(HAVE_SSE, test "x${have_sse}" = "xyes")
AM_CONDITIONAL(HAVE_PS2PDF, test "x${have_ps2pdf}" = "xyes")
AM_PROG_CC_C_O

AC_CONFIG_FILES([Makefile
                 src/Makefile
                 man/Makefile
                 examples/Makefile])
AC_OUTPUT

AC_MSG_RESULT([
  $PACKAGE $VERSION

    Target:    $host_os $host_cpu
    Compiler:  ${CC}
    CFLAGS:    ${CFLAGS} ${CPPFLAGS}
    LIBS:      ${LIBS} ${LDFLAGS}

  Continue with 'make' command
])