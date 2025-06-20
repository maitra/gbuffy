dnl Process this file with autoconf to produce a configure script.
AC_INIT(gbuffy.h)
AC_CONFIG_HEADER(config.h)
VERSION=0.2.7

AC_MSG_CHECKING(for prefix)
if test x$prefix = xNONE; then
  gbuffy_cv_prefix=$ac_default_prefix
else
  gbuffy_cv_prefix=$prefix
fi
AC_MSG_RESULT($gbuffy_cv_prefix)

AC_PROG_CC
AC_PROG_MAKE_SET
AC_PROG_INSTALL

AC_DEFINE_UNQUOTED(VERSION, "$VERSION")
AC_SUBST(VERSION)

AM_PATH_GTK(1.1.11)

gbuffy_led=no
AC_ARG_ENABLE(led, [  --enable-led            Enable LINUX specific LED support],
  [if test $enableval = yes; then
     gbuffy_led=yes
   fi
   ])

if test $gbuffy_led = yes; then
  AC_DEFINE(USE_LED)
  LED=led
  AC_SUBST(LED)
  AC_MSG_RESULT(Enabling LINUX keyboard LED blinking support)
else
  AC_MSG_RESULT(Disabling keyboard LED blinking support)
fi

enable_debug=no
AC_ARG_ENABLE(dfile, [  --enable-dfile          Enable debugfile support],
  [if test $enableval = yes; then
    enable_debug=yes
   fi])

if test $enable_debug = yes; then
  AC_DEFINE(DEBUG)
  AC_MSG_RESULT(Enabling Debug Support)
else
  AC_MSG_RESULT(Disabling Debug Support)
fi

gnome_applet=no
AC_ARG_ENABLE(applet, [  --enable-applet         Build as a GNOME Applet],
  [if test $enableval = yes; then
    gnome_applet=yes
   fi
   ])

if test $gnome_applet = yes; then
  AC_DEFINE(GNOME_APPLET)
  AC_MSG_RESULT(Building as a GNOME Applet)
  GNOME_INIT(applets)
  GAPPLET_INCLUDE="`$GNOME_CONFIG --cflags applets`"
  GAPPLET_LIBS="`$GNOME_CONFIG --libs applets`"
else
  AC_MSG_RESULT(Building as a standalone program)
fi
AC_SUBST(GAPPLET_INCLUDE)
AC_SUBST(GAPPLET_LIBS)
   
dnl gbuffy_nntp_server=no
dnl AC_ARG_ENABLE(nntp, [  --enable-nntp[=server]  Enable NNTP 'Mailbox' support],
dnl   [if test x$enableval != xyes; then
dnl      gbuffy_nntp_server=$enableval
dnl    fi
dnl    AC_DEFINE(USE_NNTP)
dnl    LIBOBJS="$LIBOBJS nntp.o"
dnl    AC_CHECK_LIB(socket, socket)
dnl    AC_CHECK_LIB(nsl, gethostbyname)
dnl    if test x$gbuffy_nntp_server != xno; then
dnl      AC_DEFINE_UNQUOTED(NNTP_SERVER, "$gbuffy_nntp_server")
dnl      AC_SUBST(NNTP_SERVER)
dnl      AC_MSG_RESULT(Using NNTP Server $gbuffy_nntp_server)
dnl    fi])

gbuffy_cv_length=yes
AC_ARG_ENABLE(length, [  --disable-length        Do NOT use Content-Length: header],
  [if test $enableval = no; then 
    gbuffy_cv_length=no;
    AC_MSG_RESULT(Disabling Content-Length: Header)
   fi])

if test $gbuffy_cv_length = yes; then
  AC_DEFINE(USE_CONTENT_LENGTH)
fi

gbuffy_cv_compface=yes
AC_CHECK_LIB(compface, uncompface)
AC_CHECK_FUNC(uncompface, gbuffy_cv_compface=no)
if test $gbuffy_cv_compface = yes; then
  LIBOBJS="$LIBOBJS libcompface/libcompface.a"
  CPPFLAGS="-I\$(srcdir)/libcompface $CPPFLAGS"
 else
  LIBS="$LIBS -lcompface"
fi

AC_HEADER_STDC

AC_CHECK_FUNCS(setpriority)
AC_REPLACE_FUNCS(utimes)

gbuffy_cv_snprintf=no
SNPRINTFOBJS=""
AC_CHECK_FUNC(snprintf, [AC_DEFINE(HAVE_SNPRINTF)], [gbuffy_cv_snprintf=yes])
AC_CHECK_FUNC(vsnprintf, [AC_DEFINE(HAVE_VSNPRINTF)], [gbuffy_cv_snprintf=yes])
if test $gbuffy_cv_snprintf = yes; then
  LIBOBJS="$LIBOBJS snprintf.o"
  SNPRINTFOBJS="snprintf.o"
fi

AC_ARG_WITH(exec-shell, [  --with-exec-shell=SHELL Specify alternate shell (ONLY if /bin/sh is broken)],
        [if test $withval != yes; then
                AC_DEFINE_UNQUOTED(EXECSHELL, "$withval")
        fi])

#ifdef HAVE_SSL
AC_ARG_WITH(ssl,
[  --with-ssl[=SSL-ROOT]   enable SSL support ], [if test "$withval" = nno; then disable_ssl=yes; else ssld="$withval"; fi])

AC_MSG_CHECKING([for OpenSSL])
CPPFLAGS_X="$CPPFLAGS"
LIBS_X="$LIBS"
cf_result=no
if test -z "$disable_ssl"; then
        for ssldir in "" /usr /usr/local /usr/local/openssl /usr/lib/openssl /usr/local/ssl /usr/local/www /usr/lib/ssl /usr/local /usr/pkg /opt /opt/openssl "$ssld"; do
                if test "$cf_result" = no; then
                        if test ! -z "$ssldir" && test "$ssldir" != /usr; then
                                LIBS="-L$ssldir/lib -lssl -lcrypto $LIBS_X"
                                CPPFLAGS="$CPPFLAGS_X -I$ssldir/include"
                        else
                                LIBS="-lssl -lcrypto $LIBS_X"
                        fi
                        AC_TRY_LINK([#include <openssl/ssl.h>], [OpenSSL_add_all_algorithms()], cf_result=yes, cf_result=no)
                        if test "$cf_result" != yes; then
                                AC_TRY_LINK([#include <openssl/ssl.h>], [SSLeay_add_ssl_algorithms()], cf_result=yes, cf_result=no)
                        fi
                fi
        done

        if test "$cf_result" != yes; then
                if test "$withval" = yes; then
                        AC_MSG_ERROR("OpenSSL not found")
                fi
                CPPFLAGS="$CPPFLAGS_X"
                LIBS="$LIBS_X"
        else
                AC_DEFINE(HAVE_SSL)
        fi
fi
AC_MSG_RESULT($cf_result)
cf_have_ssl=$cf_result;
#endif



AC_OUTPUT(Makefile libcompface/Makefile)
