AC_INIT([cpuminer], [1.0.2])

AC_PREREQ(2.52)
AC_CANONICAL_SYSTEM
AC_CONFIG_SRCDIR([cpu-miner.c])
AM_INIT_AUTOMAKE([gnu])
AC_CONFIG_MACRO_DIR([m4])
AC_CONFIG_HEADERS([cpuminer-config.h])

dnl Make sure anyone changing configure.ac/Makefile.am has a clue
AM_MAINTAINER_MODE

dnl Checks for programs
AC_PROG_CC
AC_PROG_GCC_TRADITIONAL
AM_PROG_CC_C_O
AM_PROG_AS
AC_PROG_RANLIB

dnl Checks for header files.
AC_HEADER_STDC
AC_CHECK_HEADERS(syslog.h)

AC_FUNC_ALLOCA

case $target in
  *-*-mingw*)
    have_x86_64=false
    have_win32=true
    PTHREAD_FLAGS=""
    ;;
  x86_64-*)
    have_x86_64=true
    have_win32=false
    PTHREAD_FLAGS="-pthread"
    ;;
  *)
    have_x86_64=false
    have_win32=false
    PTHREAD_FLAGS="-pthread"
    ;;
esac


AC_CHECK_LIB(jansson, json_loads, request_jansson=false, request_jansson=true)
AC_CHECK_LIB(pthread, pthread_create, PTHREAD_LIBS=-lpthread)
AC_CHECK_LIB(tcmalloc, malloc, TMALLOC_LIBS=-ltcmalloc, AC_MSG_WARN([google-perftools is not installed. DRAMA.WAV!]))

AM_CONDITIONAL([WANT_JANSSON], [test x$request_jansson = xtrue])
AM_CONDITIONAL([HAVE_WINDOWS], [test x$have_win32 = xtrue])
AM_CONDITIONAL([HAVE_x86_64], [test x$have_x86_64 = xtrue])



AC_ARG_WITH([cpuprofiler],[AS_HELP_STRING([--with-cpuprofiler],[enable google-perftools cpuprofiler, default disabled])],[CPUPROF_LIBS='-lprofiler'])
dnl AX_EXT()
dnlif HAVE_SSE2
dnlthen 
dnlAC_MSG_WARN([Your cpu is sse2 capable!!!!])
dnlfi

if test x$request_jansson = xtrue
then
	JANSSON_LIBS="compat/jansson/libjansson.a"
else
	JANSSON_LIBS=-ljansson
fi

PKG_PROG_PKG_CONFIG()

LIBCURL_CHECK_CONFIG(, 7.10.1, ,
  [AC_MSG_ERROR([Missing required libcurl >= 7.10.1])])

AC_SUBST(JANSSON_LIBS)
AC_SUBST(PTHREAD_FLAGS)
AC_SUBST(PTHREAD_LIBS)
AC_SUBST(TMALLOC_LIBS)
AC_SUBST(CPUPROF_LIBS)

AC_CONFIG_FILES([
	Makefile
	compat/Makefile
	compat/jansson/Makefile
	])
AC_OUTPUT

