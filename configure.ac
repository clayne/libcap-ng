
dnl
define([AC_INIT_NOTICE],
[### Generated automatically using autoconf version] AC_ACVERSION [
### Copyright 2009-2024 Steve Grubb <sgrubb@redhat.com>
###
### Permission is hereby granted, free of charge, to any person obtaining a
### copy of this software and associated documentation files (the "Software"),
### to deal in the Software without restriction, including without limitation
### the rights to use, copy, modify, merge, publish, distribute, sublicense,
### and/or sell copies of the Software, and to permit persons to whom the
### Software is furnished to do so, subject to the following conditions:
###
### The above copyright notice and this permission notice shall be included
### in all copies or substantial portions of the Software.
###
### THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
### IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
### FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL
### THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR
### OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE,
### ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR
### OTHER DEALINGS IN THE SOFTWARE.
###
### For usage, run `./configure --help'
### For more detailed information on installation, read the file `INSTALL'.
###
### If configuration succeeds, status is in the file `config.status'.
### A log of configuration tests is in `config.log'.
])

AC_REVISION($Revision: 1.3 $)dnl
AC_INIT([libcap-ng],[0.8.6])
AC_PREREQ([2.50])dnl
AC_CONFIG_HEADERS([config.h])
AC_CONFIG_MACRO_DIR([m4])

echo Configuring libcap-ng $VERSION

AC_CANONICAL_TARGET
AM_INIT_AUTOMAKE
LT_INIT
AC_SUBST(LIBTOOL_DEPS)

echo .
echo Checking for programs

AC_PROG_CC
AC_PROG_INSTALL
AC_PROG_AWK

echo .
echo Checking for header files
AC_CHECK_HEADERS(linux/capability.h, [], [AC_MSG_ERROR(linux/capability.h is required in order to build libcap-ng.)])
AC_CHECK_HEADERS(sys/xattr.h, [], [
	AC_CHECK_HEADERS(attr/xattr.h, [], [AC_MSG_WARN(attr/xattr.h not found, disabling file system capabilities.)])
	])
AC_CHECK_HEADERS(linux/securebits.h, [], [])
AC_CHECK_HEADERS(pthread.h,
	[AC_SEARCH_LIBS(pthread_atfork, pthread)],
	[AC_MSG_WARN(pthread.h not found, disabling pthread_atfork.)])
AC_CHECK_HEADERS(sys/vfs.h, [
	AC_CHECK_HEADERS(linux/magic.h, [], [AC_MSG_WARN(linux/magic.h is required in order to verify procfs.)])
	], [AC_MSG_WARN(sys/vfs.h is required in order to verify procfs.)])

AC_ARG_WITH([capability_header],
        [AS_HELP_STRING([--with-capability_header=path : path to capability.h])],
        [CAPABILITY_HEADER=$withval],
        [CAPABILITY_HEADER=/usr/include/linux/capability.h])
AC_SUBST(CAPABILITY_HEADER)

ALLWARNS=""
ALLDEBUG="-g"
OPT="-O"
if test x"$GCC" = x"yes"; then
  OPT="-O2 -pipe"
  case "$target" in
    *linux*)
             ALLWARNS="-W -Wall -Wundef -Wpointer-arith -Wcast-align \
-Wwrite-strings -Waggregate-return -Wstrict-prototypes \
-Wmissing-prototypes -Wmissing-declarations -Wredundant-decls \
-Wnested-externs -Winline -Wfloat-equal -Wchar-subscripts"
             ;;
    esac
fi

ALLDEBUG="-g"
AC_ARG_WITH(debug,
[  --with-debug            turn on debugging [[default=no]]],
[
if test "x${withval}" = xyes; then
        DEBUG="$ALLDEBUG"
        OPT="-O"
        AM_CONDITIONAL(DEBUG, true)
else
        DEBUG="-DNDEBUG"
        AM_CONDITIONAL(DEBUG, false)
fi
],
[ DEBUG="-DNDEBUG"; AM_CONDITIONAL(DEBUG, false) ])

AC_MSG_CHECKING(__attr_dealloc_free support)
AC_COMPILE_IFELSE(
  [AC_LANG_SOURCE(
    [[
      #include <stdio.h>
      extern char *test(const char *name) __attr_dealloc_free;
      int main(void) { return 0; }]])],
  [DEALLOC="yes"],
  [DEALLOC="no"]
)
AC_MSG_RESULT($DEALLOC)

AC_ARG_WITH(warn,
[  --with-warn             turn on warnings [[default=yes]]],
[
if test "x${withval}" = xyes; then
        WARNS="$ALLWARNS"
else
        WARNS=""
fi
],WARNS="$ALLWARNS")
AC_SUBST(DEBUG)

WFLAGS=""
AC_MSG_CHECKING(for -Wformat-truncation)
TMPCFLAGS="${CFLAGS}"
CFLAGS="${CFLAGS} -Wformat-truncation"
AC_LINK_IFELSE([AC_LANG_PROGRAM([[]], [[]])],[WFLAGS="-Wno-format-truncation"
	AC_MSG_RESULT(yes)],
	[AC_MSG_RESULT(no)])
CFLAGS="${TMPCFLAGS}"
AC_SUBST(WFLAGS)

AC_CHECK_HEADER(sys/syscall.h,
	[AC_DEFINE([HAVE_SYSCALL_H], [1],
	[Define to 1 if you have <sys/syscall.h>.])], [],)

dnl; This is to record in the build logs what options are being taken
AC_CHECK_DECLS([PR_CAP_AMBIENT], [], [], [[#include <sys/prctl.h>]])
AC_CHECK_DECLS([VFS_CAP_REVISION_2], [], [], [[#include <linux/capability.h>]])
AC_CHECK_DECLS([VFS_CAP_REVISION_3], [], [], [[#include <linux/capability.h>]])
AC_CHECK_DECLS([CAP_EPOLLWAKEUP], [], [], [[#include <linux/capability.h>]])
AC_CHECK_DECLS([CAP_WAKE_ALARM], [], [], [[#include <linux/capability.h>]])
AC_CHECK_DECLS([CAP_BLOCK_SUSPEND], [], [], [[#include <linux/capability.h>]])
AC_CHECK_DECLS([CAP_AUDIT_READ], [], [], [[#include <linux/capability.h>]])
AC_CHECK_DECLS([CAP_PERFMON], [], [], [[#include <linux/capability.h>]])
AC_CHECK_DECLS([CAP_BPF], [], [], [[#include <linux/capability.h>]])
AC_CHECK_DECLS([CAP_CHECKPOINT_RESTORE], [], [], [[#include <linux/capability.h>]])

dnl only build libdrop_ambient if support for ambient capabilities was found (which is normal)
if test x"${ac_cv_have_decl_PR_CAP_AMBIENT}" = x"no" ; then
    AC_MSG_WARN("PR_CAP_AMBIENT not available - libdrop_ambient will not be built")
fi
AM_CONDITIONAL(BUILD_LIBDROP_AMBIENT, test x"${ac_cv_have_decl_PR_CAP_AMBIENT}" = x"yes")

AC_CHECK_PROG(swig_found, swig, yes, no)
if test x"${swig_found}" = x"no" ; then
	AC_MSG_WARN("Swig not found - python bindings will not be made")
fi
AM_CONDITIONAL(HAVE_SWIG, test x"${swig_found}" = x"yes")

withval=""
AC_MSG_CHECKING(whether to create python3 bindings)
AC_ARG_WITH(python3,
AS_HELP_STRING([--with-python3],[enable building python3 bindings]),
use_python3=$withval,
use_python3=auto)
if test x$use_python3 = xno ; then
	AC_MSG_RESULT(no)
else
	AC_MSG_RESULT(investigating)
	AC_PATH_PROG([use_python3], [python3-config], [no])
	if test ${use_python3} = no ; then
		if test ${withval} = yes ; then
			echo "Python3 bindings were selected but python3-config was not found."
			echo "Please ensure that it's installed or pass --without-python3 to ./configure"
			exit 1
		fi
		echo "Python3 bindings will NOT be built"
	else
	echo "Python3 bindings WILL be built"
	use_python3=yes
	AM_PATH_PYTHON([3.1],, [:])
	PYTHON3_CFLAGS=`python3-config --cflags 2> /dev/null`
	PYTHON3_LIBS=`python3-config --libs 2> /dev/null`
	PYTHON3_INCLUDES=`python3-config --includes 2> /dev/null`
	AC_SUBST(PYTHON3_CFLAGS)
	AC_SUBST(PYTHON3_LIBS)
	AC_SUBST(PYTHON3_INCLUDES)
	fi
fi
AM_CONDITIONAL(USE_PYTHON3, test ${use_python3} = "yes")

AC_CONFIG_FILES([Makefile src/Makefile src/libcap-ng.pc src/test/Makefile 
	bindings/Makefile
        bindings/python3/Makefile bindings/src/Makefile
        bindings/python3/test/Makefile bindings/test/Makefile utils/Makefile
	m4/Makefile docs/Makefile])
AC_OUTPUT

echo .
echo "

  libcap-ng Version:      $VERSION
  Target:                 $target
  Installation prefix:    $prefix
  Compiler:               $CC
  Compiler flags:
`echo $CFLAGS | fmt -w 50 | sed 's,^,                          ,'`
  __attr_dealloc_free support:  $DEALLOC
"
