#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

AC_PREREQ([2.68])
AC_INIT([polarssl], [1.3.9], [polarssl_maintainer@polarssl.org])
AC_CONFIG_SRCDIR([library/ssl_cache.c])

m4_define([polarssl_ltversion], [7:0:0])
AC_CONFIG_MACRO_DIR([m4])
AC_CANONICAL_HOST
AC_USE_SYSTEM_EXTENSIONS

AM_INIT_AUTOMAKE
AM_SILENT_RULES(yes)

# Checks for programs.
AC_PROG_CC

#
# Libtool
#
ifdef(
	[LT_INIT],
	[
		LT_INIT([win32-dll])
		LT_LANG([Windows Resource])
	],
	[
		AC_LIBTOOL_WIN32_DLL
		AC_LIBTOOL_RC
		AC_PROG_LIBTOOL
	]
)

dnl Workaround for mingw, whose compiler doesn't check in /usr/include ...
case "$target_os" in
	*mingw*)
		if test "$prefix" = "/usr" ; then
			CPPFLAGS="$CPPFLAGS -I/usr/include"
			LDFLAGS="$LDFLAGS -L/usr/lib"
		fi
	;;
esac


AC_DEFINE_UNQUOTED([TARGET_ALIAS], ["${host}"], [A string representing our host])
case "$host" in
	*-*-linux*)
		AC_DEFINE([TARGET_LINUX], [1], [Are we running on Linux?])
		AC_DEFINE_UNQUOTED([TARGET_PREFIX], ["L"], [Target prefix])
		;;
	*-*-solaris*)
		AC_DEFINE([TARGET_SOLARIS], [1], [Are we running on Solaris?])
		AC_DEFINE_UNQUOTED([TARGET_PREFIX], ["S"], [Target prefix])
		;;
	*-*-openbsd*)
		AC_DEFINE([TARGET_OPENBSD], [1], [Are we running on OpenBSD?])
		AC_DEFINE_UNQUOTED([TARGET_PREFIX], ["O"], [Target prefix])
		;;
	*-*-freebsd*)
		AC_DEFINE([TARGET_FREEBSD], [1], [Are we running on FreeBSD?])
		AC_DEFINE_UNQUOTED([TARGET_PREFIX], ["F"], [Target prefix])
		;;
	*-*-netbsd*)
		AC_DEFINE([TARGET_NETBSD], [1], [Are we running NetBSD?])
		AC_DEFINE_UNQUOTED([TARGET_PREFIX], ["N"], [Target prefix])
		;;
	*-*-darwin*)
		AC_DEFINE([TARGET_DARWIN], [1], [Are we running on Mac OS X?])
		AC_DEFINE_UNQUOTED([TARGET_PREFIX], ["M"], [Target prefix])
		have_tap_header="yes"
		dnl some Mac OS X tendering (we use vararg macros...)
		CPPFLAGS="$CPPFLAGS -no-cpp-precomp"
		;;
	*-mingw*)
		AC_DEFINE([TARGET_WIN32], [1], [Are we running WIN32?])
		AC_DEFINE_UNQUOTED([TARGET_PREFIX], ["W"], [Target prefix])
		CPPFLAGS="${CPPFLAGS} -DWIN32_LEAN_AND_MEAN"
		CPPFLAGS="${CPPFLAGS} -DNTDDI_VERSION=NTDDI_WINXP -D_WIN32_WINNT=_WIN32_WINNT_WINXP"
		WIN32="yes"
		;;
	*-*-dragonfly*)
		AC_DEFINE([TARGET_DRAGONFLY], [1], [Are we running on DragonFlyBSD?])
		AC_DEFINE_UNQUOTED([TARGET_PREFIX], ["D"], [Target prefix])
		;;
	*-aix*)
		AC_DEFINE([TARGET_AIX], [1], [Are we running AIX?])
		AC_DEFINE_UNQUOTED([TARGET_PREFIX], ["A"], [Target prefix])
		ROUTE="/usr/sbin/route"
		have_tap_header="yes"
		ac_cv_header_net_if_h="no"	# exists, but breaks things
		;;
	*)
		AC_DEFINE_UNQUOTED([TARGET_PREFIX], ["X"], [Target prefix])
		have_tap_header="yes"
		;;
esac

AC_ARG_ENABLE(
	[pkcs11],
	[AS_HELP_STRING([--enable-pkcs11], [enable pkcs11 support @<:@default=no@:>@])],
	,
	[enable_pkcs11="no"]
)

PKG_PROG_PKG_CONFIG

# Checks for libraries.

AC_CHECK_LIB([crypto], [crypto_init])
# FIXME: Replace `main' with a function in `-lssl':
AC_CHECK_LIB([ssl_library_init], [main])

# FIXME: Replace `main' with a function in `-lz':
#AC_CHECK_LIB([z], [main])

# Checks for header files.
AC_CHECK_HEADERS([arpa/inet.h fcntl.h float.h inttypes.h netdb.h netinet/in.h stdint.h stdlib.h string.h sys/socket.h sys/time.h unistd.h])

# Checks for typedefs, structures, and compiler characteristics.
AC_HEADER_STDBOOL
AC_C_INLINE
AC_TYPE_INT16_T
AC_TYPE_INT32_T
AC_TYPE_INT64_T
AC_TYPE_OFF_T
AC_TYPE_SIZE_T
AC_TYPE_UINT16_T
AC_TYPE_UINT32_T
AC_TYPE_UINT64_T

# Checks for library functions.
AC_FUNC_FORK
AC_CHECK_FUNCS([alarm gethostbyname gethostname gettimeofday memmove memset select socket strcasecmp strchr strstr])

PKG_CHECK_MODULES(
	[PKCS11_HELPER],
	[libpkcs11-helper-1 >= 1.11],
	[have_pkcs11_helper="yes"],
	[]
)

if test "${enable_pkcs11}" = "yes"; then
	test "${have_pkcs11_helper}" != "yes" && AC_MSG_ERROR([PKCS11 enabled but libpkcs11-helper is missing])
	OPTIONAL_PKCS11_HELPER_CFLAGS="${PKCS11_HELPER_CFLAGS}"
	OPTIONAL_PKCS11_HELPER_LIBS="${PKCS11_HELPER_LIBS}"
	if [[ -x "scripts/config.pl" ]]
	then
		scripts/config.pl set POLARSSL_PKCS11_C
	else
		chmod x scripts/config.pl
		scripts/config.pl set POLARSSL_PKCS11_C
	fi
	
fi

AC_SUBST([OPTIONAL_PKCS11_HELPER_CFLAGS])
AC_SUBST([OPTIONAL_PKCS11_HELPER_LIBS])
AC_SUBST([POLARSSL_LTVERSION], polarssl_ltversion)

AM_CONDITIONAL([WIN32], [test "${WIN32}" = "yes"])

AC_CONFIG_FILES([Makefile
	include/Makefile
	include/polarssl/Makefile
	library/Makefile
])
AC_OUTPUT
