AC_PREREQ([2.64])

AC_INIT([units],
        [208],
        [http://bugs.are.everywhere],
        [units],
        [http://www.example.com])

AC_PREFIX_DEFAULT([/usr])
AM_INIT_AUTOMAKE([foreign 1.11 silent-rules tar-pax no-dist-gzip dist-xz subdir-objects])
AM_SILENT_RULES([yes])

AC_PROG_MKDIR_P
AC_PATH_PROG([M4], [m4])
AC_PROG_SED
AC_PROG_LN_S

AC_PATH_PROG([QUOTAON], [quotaon], [/usr/sbin/quotaon], [$PATH:/usr/sbin:/sbin])
AC_PATH_PROG([QUOTACHECK], [quotacheck], [/usr/sbin/quotacheck], [$PATH:/usr/sbin:/sbin])
AC_PATH_PROG([KILL], [kill], [/usr/bin/kill], [$PATH:/usr/sbin:/sbin])
AC_PATH_PROG([KMOD], [kmod], [/usr/bin/kmod], [$PATH:/usr/sbin:/sbin])

PKG_CHECK_MODULES(DBUS, [dbus-1 >= 1.3.2])

AC_ARG_WITH([rootprefix],
         AS_HELP_STRING([--with-rootprefix=DIR], [rootfs directory prefix for config files and kernel modules]),
         [], [with_rootprefix=${ac_default_prefix}])
AC_SUBST([rootprefix], [$with_rootprefix])

AC_ARG_WITH([dbussessionservicedir],
        AS_HELP_STRING([--with-dbussessionservicedir=DIR], [D-Bus session service directory]),
        [],
        [with_dbussessionservicedir=$($PKG_CONFIG --variable=session_bus_services_dir dbus-1)])
AC_SUBST([dbussessionservicedir], [$with_dbussessionservicedir])

have_quotacheck=no
AC_ARG_ENABLE(quotacheck, AS_HELP_STRING([--disable-quotacheck], [disable quotacheck tools]))
if test "x$enable_quotacheck" != "xno"; then
        have_quotacheck=yes
fi
AM_CONDITIONAL(ENABLE_QUOTACHECK, [test "$have_quotacheck" = "yes"])

have_kmod=no
AC_ARG_ENABLE(kmod, AS_HELP_STRING([--disable-kmod], [disable loadable modules support]))
if test "x$enable_kmod" != "xno"; then
        PKG_CHECK_EXISTS([ libkmod ], have_kmod=yes, have_kmod=no)
        if test "x$have_kmod" = "xyes"; then
                PKG_CHECK_MODULES(KMOD, [ libkmod >= 14 ],
                        [AC_DEFINE(HAVE_KMOD, 1, [Define if kmod is available])],
                        AC_MSG_ERROR([*** kmod version >= 14 not found]))
        fi
        if test "x$have_kmod" = xno -a "x$enable_kmod" = xyes; then
                AC_MSG_ERROR([*** kmod support requested, but libraries not found])
        fi
fi
AM_CONDITIONAL(HAVE_KMOD, [test "$have_kmod" = "yes"])

AC_OUTPUT([Makefile])
AC_MSG_RESULT([
        $PACKAGE_NAME $VERSION

        prefix:                  ${prefix}
        rootprefix:              ${rootprefix}
        D-Bus session dir:       ${with_dbussessionservicedir}
        quotacheck:              ${have_quotacheck}
        kmod:                    ${have_kmod}
])
