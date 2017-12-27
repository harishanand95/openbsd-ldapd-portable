AC_INIT(ldapd, 0.1p, martin@bzero.se)
AC_PROG_CC
AC_PROG_YACC
AM_INIT_AUTOMAKE([foreign])

AC_CONFIG_LIBOBJ_DIR([compat])

AC_LIBOBJ([base64])
AC_LIBOBJ([imsg])
AC_LIBOBJ([imsg-buffer])

AC_CHECK_FUNCS([setproctitle setresgid auth_userokay getpeereid])
AC_REPLACE_FUNCS([memrchr strlcpy strlcat strtonum arc4random])
AC_CHECK_FUNC(strvisx, , [AC_LIBOBJ(vis)])
AC_CHECK_FUNC(arc4random_buf, , [AC_LIBOBJ(arc4random)])

AC_SEARCH_LIBS(crypt, [crypt], , [
	AC_MSG_ERROR([crypt library not found])
])

AC_CHECK_LIB([crypto], [SHA1_Update], [LIBS="$LIBS -lcrypto"], [
	AC_MSG_ERROR([crypto library not found])
])

AC_CHECK_LIB([z], [compress2], [LIBS="$LIBS -lz"], [
	AC_MSG_ERROR([gzip library not found])
])

AC_CHECK_LIB([ssl], [SSL_CTX_check_private_key], [LIBS="$LIBS -lssl"], [
	AC_MSG_ERROR([SSL library not found])
])


AC_CHECK_HEADERS([net/if_types.h net/if_media.h net/if_dl.h sys/file.h])

dnl ss_len checks copied from NetBSD CVS:src/dist/ntp
AC_CACHE_CHECK([for ss_len field in struct sockaddr_storage],
		ac_cv_have_ss_len_in_struct_ss, [
	AC_TRY_COMPILE(
		[
#include <sys/types.h>
#include <sys/socket.h>
		],
		[ struct sockaddr_storage s; s.ss_len = 1; ],
		[ ac_cv_have_ss_len_in_struct_ss="yes" ],
		[ ac_cv_have_ss_len_in_struct_ss="no" ],
	)
])
if test "x$ac_cv_have_ss_len_in_struct_ss" = "xyes" ; then
	AC_DEFINE(HAVE_SS_LEN_IN_SS, 1, [Does struct sockaddr_storage have ss_len?])
fi

AC_CACHE_CHECK([for __ss_len field in struct sockaddr_storage],
		ac_cv_have___ss_len_in_struct_ss, [
	AC_TRY_COMPILE(
		[
#include <sys/types.h>
#include <sys/socket.h>
		],
		[ struct sockaddr_storage s; s.__ss_len = 1; ],
		[ ac_cv_have___ss_len_in_struct_ss="yes" ],
		[ ac_cv_have___ss_len_in_struct_ss="no" ]
	)
])
if test "x$ac_cv_have___ss_len_in_struct_ss" = "xyes" ; then
	AC_DEFINE(HAVE___SS_LEN_IN_SS, 1, [Does struct sockaddr_storage have __ss_len?])
fi

AH_VERBATIM([X_HAVE_SS_LEN_IN_SS],
[/* Handle ss_len */
#if !defined(HAVE_SS_LEN_IN_SS) && defined(HAVE___SS_LEN_IN_SS)
# define ss_len __ss_len
#endif /* !defined(HAVE_SS_LEN_IN_SS) && defined(HAVE_SA_LEN_IN_SS) */])




dnl libevent stuff copied from memcached
trylibeventdir=""
AC_ARG_WITH(libevent,
       [  --with-libevent=PATH     Specify path to libevent installation ],
       [
                if test "x$withval" != "xno" ; then
                        trylibeventdir=$withval
                fi
       ]
)

dnl ------------------------------------------------------
dnl libevent detection.  swiped from Tor.  modified a bit.

LIBEVENT_URL=http://www.monkey.org/~provos/libevent/

AC_CACHE_CHECK([for libevent directory], ac_cv_libevent_dir, [
  saved_LIBS="$LIBS"
  saved_LDFLAGS="$LDFLAGS"
  saved_CPPFLAGS="$CPPFLAGS"
  le_found=no
  for ledir in $trylibeventdir "" $prefix /usr/local /opt/local ; do
    LDFLAGS="$saved_LDFLAGS"
    LIBS="$saved_LIBS -levent -lbsd"

    # Skip the directory if it isn't there.
    if test ! -z "$ledir" -a ! -d "$ledir" ; then
       continue;
    fi
    if test ! -z "$ledir" ; then
      if test -d "$ledir/lib" ; then
        LDFLAGS="-L$ledir/lib $LDFLAGS"
      else
        LDFLAGS="-L$ledir $LDFLAGS"
      fi
      if test -d "$ledir/include" ; then
        CPPFLAGS="-I$ledir/include $CPPFLAGS"
      else
        CPPFLAGS="-I$ledir $CPPFLAGS"
      fi
    fi
    # Can I compile and link it?
    AC_TRY_LINK([#include <sys/time.h>
#include <sys/types.h>
#include <event.h>], [ event_init(); ],
       [ libevent_linked=yes ], [ libevent_linked=no ])
    if test $libevent_linked = yes; then
       if test ! -z "$ledir" ; then
         ac_cv_libevent_dir=$ledir
       else
         ac_cv_libevent_dir="(system)"
       fi
       le_found=yes
       break
    fi
  done
  LIBS="$saved_LIBS"
  LDFLAGS="$saved_LDFLAGS"
  CPPFLAGS="$saved_CPPFLAGS"
  if test $le_found = no ; then
    AC_MSG_ERROR([libevent is required.  You can get it from $LIBEVENT_URL

      If it's already installed, specify its path using --with-libevent=/dir/
])
  fi
])
LIBS="$LIBS -levent -lbsd"
if test $ac_cv_libevent_dir != "(system)"; then
  if test -d "$ac_cv_libevent_dir/lib" ; then
    LDFLAGS="-L$ac_cv_libevent_dir/lib $LDFLAGS"
    le_libdir="$ac_cv_libevent_dir/lib"
  else
    LDFLAGS="-L$ac_cv_libevent_dir $LDFLAGS"
    le_libdir="$ac_cv_libevent_dir"
  fi
  if test -d "$ac_cv_libevent_dir/include" ; then
    CPPFLAGS="-I$ac_cv_libevent_dir/include $CPPFLAGS"
  else
    CPPFLAGS="-I$ac_cv_libevent_dir $CPPFLAGS"
  fi
fi



AC_CONFIG_HEADERS([config.h])
AC_CONFIG_FILES([Makefile ldapd/Makefile ldapctl/Makefile compat/Makefile])
AC_OUTPUT

