dnl configure.ac for netatalk

AC_INIT([netatalk],m4_normalize(m4_include([VERSION])))
AC_CANONICAL_TARGET
AM_INIT_AUTOMAKE([subdir-objects])
AC_CONFIG_HEADERS([config.h])
AC_CONFIG_MACRO_DIRS([macros])
AM_MAINTAINER_MODE([enable])
AM_SILENT_RULES([yes])

NETATALK_VERSION=`cat $srcdir/VERSION`
AC_SUBST(NETATALK_VERSION)

dnl Checks for programs.
AC_PROG_CC
AC_PROG_PERL
LT_INIT

AC_SYS_LARGEFILE([], AC_MSG_ERROR([AFP 3.x support requires Large File Support.]))

AC_DEFINE(OPEN_NOFOLLOW_ERRNO, ELOOP, errno returned by open with O_NOFOLLOW)

dnl Special checks
ac_neta_haveatfuncs=yes
AC_CHECK_FUNCS(openat renameat fstatat unlinkat, , ac_neta_haveatfuncs=no)
if test x"$ac_neta_haveatfuncs" = x"yes" ; then
   AC_DEFINE([_ATFILE_SOURCE], 1, AT file source)
   AC_DEFINE([HAVE_ATFUNCS], 1, whether at funcs are available)
fi

dnl Search for necessary libraries
AC_SEARCH_LIBS(gethostbyname, nsl)
AC_SEARCH_LIBS(connect, socket)
AC_CHECK_FUNCS(getifaddrs) dnl comes after gethostbyname and connect so it picks up the libs

dnl 64bit platform check
AC_NETATALK_64BIT_LIBS

dnl --with-pkgconfdir check to change configuration directory location
AC_NETATALK_CONFIG_DIRS

dnl Check whether to enable debug code
AC_NETATALK_DEBUG

dnl Check whether to disable tickle SIGALARM stuff, which eases debugging
AC_NETATALK_DEBUGGING

dnl	Check for libiconv support
AC_NETATALK_CHECK_ICONV

dnl Check for optional Zeroconf support
AC_NETATALK_ZEROCONF

dnl Check for CNID database backends
AC_NETATALK_CNID([bdb_required=yes],[bdb_required=no])

dnl Check for PAM libs
AC_NETATALK_PATH_PAM

dnl Path where UAM modules shall be installed
AC_ARG_WITH(uams-path, [  --with-uams-path=PATH   path to UAMs [[$libdir/netatalk/]]], [uams_path="$withval/"], [uams_path="$libdir/netatalk/"])

dnl Check for libgcrypt, if found enables DHX2 UAM
AC_NETATALK_PATH_LIBGCRYPT([1:1.2.3])

dnl Check for openssl, if found enables DHX UAM
AC_NETATALK_PATH_SSL

dnl Check for Berkeley DB library
AC_NETATALK_PATH_BDB

dnl Check for crypt
AC_NETATALK_CRYPT

dnl Check for building Kerberos V UAM module
AC_NETATALK_KRB5_UAM

dnl Check for Kerberos 5 support
AC_NETATALK_KERBEROS

dnl Check for overwrite the config files or not
AC_NETATALK_OVERWRITE_CONFIG

dnl Check for LDAP support, for client-side ACL visibility
AC_NETATALK_LDAP

dnl Check for ACL support
AC_NETATALK_ACL

dnl Check for Extended Attributes support
AC_NETATALK_EXTENDED_ATTRIBUTES

dnl Check for libevent
AC_NETATALK_LIBEVENT

dnl netatalk lockfile path
AC_NETATALK_LOCKFILE

dnl Check for Docbook and build documentation if found
AX_CHECK_DOCBOOK

CFLAGS="-I\$(top_srcdir)/include -I\$(top_builddir)/include -I\$(top_srcdir) $CFLAGS"
UAMS_PATH="${uams_path}"

AC_SUBST(LIBS)
AC_SUBST(CFLAGS)
AC_SUBST(UAMS_PATH)

AM_CONDITIONAL(HAVE_LIBGCRYPT, test x$neta_cv_have_libgcrypt = xyes)
AM_CONDITIONAL(HAVE_OPENSSL, test x$neta_cv_have_openssl = xyes)
AM_CONDITIONAL(HAVE_ACLS, test x"$ac_cv_have_acls" = x"yes")
AM_CONDITIONAL(HAVE_LDAP, test x"$netatalk_cv_ldap" = x"yes")
AM_CONDITIONAL(USE_DHX, test x$neta_cv_compile_dhx = xyes)
AM_CONDITIONAL(USE_DHX2, test x$neta_cv_compile_dhx2 = xyes)
AM_CONDITIONAL(USE_PAM_SO, test x$use_pam_so = xyes)
AM_CONDITIONAL(USE_PAM, test x$netatalk_cv_install_pam = xyes)
AM_CONDITIONAL(BUILD_PAM, test x$compile_pam = xyes)
AM_CONDITIONAL(DEFAULT_HOOK, test x$neta_cv_have_libgcrypt != xyes && test x$neta_cv_have_openssl != xyes)
AM_CONDITIONAL(USE_BDB, test x$bdb_required = xyes)
AM_CONDITIONAL(HAVE_ATFUNCS, test x"$ac_neta_haveatfuncs" = x"yes")

dnl Enable silent Automake rules if present                                                                           
m4_ifdef([AM_SILENT_RULES],[AM_SILENT_RULES([yes])])

dnl --------------------- generate files
AC_CONFIG_FILES([Makefile
	bin/Makefile
	bin/ad/Makefile
	bin/cnid/Makefile
	bin/cnid/cnid2_create
	bin/megatron/Makefile
	bin/misc/Makefile
	config/Makefile
	config/pam/Makefile
	contrib/Makefile
	contrib/macusers/Makefile
	contrib/macusers/macusers
	contrib/shell_utils/Makefile
	contrib/shell_utils/apple_dump
	contrib/shell_utils/asip-status.pl
	distrib/Makefile
	distrib/config/Makefile
	distrib/config/netatalk-config
	distrib/m4/Makefile
	doc/Makefile
    doc/html.xsl
    doc/man.xsl
    doc/manual/Makefile
    doc/manual/manual.xml
    doc/manpages/Makefile
    doc/manpages/man1/Makefile
    doc/manpages/man5/Makefile
    doc/manpages/man8/Makefile
	etc/Makefile
	etc/afpd/Makefile
	etc/cnid_dbd/Makefile
	etc/uams/Makefile
	include/Makefile
	include/atalk/Makefile
	libatalk/Makefile
	libatalk/acl/Makefile
	libatalk/adouble/Makefile
	libatalk/bstring/Makefile
	libatalk/cnid/Makefile
	libatalk/cnid/last/Makefile
	libatalk/cnid/dbd/Makefile
	libatalk/cnid/mysql/Makefile
	libatalk/dsi/Makefile
	libatalk/iniparser/Makefile
	libatalk/talloc/Makefile
	libatalk/unicode/Makefile
	libatalk/unicode/charsets/Makefile
	libatalk/util/Makefile
	libatalk/vfs/Makefile
	macros/Makefile
	man/Makefile
	man/man1/Makefile
    man/man1/ad.1
    man/man1/afpldaptest.1
    man/man1/apple_dump.1
    man/man1/asip-status.pl.1
    man/man1/dbd.1
    man/man1/macusers.1
    man/man1/megatron.1
    man/man1/netatalk-config.1
    man/man1/uniconv.1
	man/man5/Makefile
    man/man5/afp.conf.5
    man/man5/afp_signature.conf.5
    man/man5/afp_voluuid.conf.5
    man/man5/extmap.conf.5
	man/man8/Makefile
    man/man8/afpd.8
    man/man8/cnid_dbd.8
    man/man8/cnid_metad.8
	])
AC_CONFIG_COMMANDS([default],[chmod a+x distrib/config/netatalk-config contrib/shell_utils/apple_*
],[])
AC_OUTPUT

AC_NETATALK_LIBS_SUMMARY
AC_NETATALK_CONFIG_SUMMARY
