AC_PREREQ([2.68])
AC_INIT([telegram], [0.1])
AC_CONFIG_SRCDIR([config.h.in])
AC_CONFIG_HEADERS([config.h])

m4_include([ax_lua.m4])

# Checks for programs.
AC_PROG_CC

# Checks for libraries.
AC_CHECK_LIB([m], [sqrt])
AC_CHECK_LIB([crypto], [AES_set_encrypt_key])

EXTRA_LIBS=""

BT=
AC_CHECK_FUNC([backtrace],
  [
    BT=1
  ],
  [
  ]
)

if test "x$BT" = "x" ; then
  AC_CHECK_LIB([execinfo], [backtrace],
  [ EXTRA_LIBS="${EXTRA_LIBS} -lexecinfo" ; ],
  [ CFLAGS="${CFLAGS} -DNO_BACKTRACE" ; ])
fi

AC_CHECK_LIB([readline], [rl_save_prompt], 
  [
    AC_DEFINE([READLINE_GNU], [1], [Use gnu libreadline])
    [ EXTRA_LIBS="${EXTRA_LIBS} -lreadline" ; ]
  ],
  [
    AC_CHECK_LIB([edit], [rl_set_prompt])
    AC_DEFINE([READLINE_EDIT], [1], [Use libedit])
    [ EXTRA_LIBS="${EXTRA_LIBS} -ledit" ; ]
  ]
)

AC_CHECK_LIB([rt], [clock_gettime], 
  [
    [ EXTRA_LIBS="${EXTRA_LIBS} -lrt" ; ]
  ],
  [
  ]
)

AC_MSG_CHECKING([for libconfig])
AC_ARG_ENABLE(libconfig,[--enable-libconfig/--disable-libconfig],
  [ 
  if test "x$enableval" = "xno" ; then
    AC_MSG_RESULT([disabled])
  else
    AC_MSG_RESULT([enabled])
    AC_CHECK_LIB([config],[config_init],AC_DEFINE(ENABLE_LIBCONFIG,1,[enable libconfig]),AC_MSG_ERROR([No libconfig found. Try --disable-libconfig]))
    [EXTRA_LIBS="${EXTRA_LIBS} -lconfig" ; ]
  fi 
  ],[
    AC_MSG_RESULT([enabled])
    AC_CHECK_LIB([config],[config_init],AC_DEFINE(ENABLE_LIBCONFIG,1,[enable libconfig]),AC_MSG_ERROR([No libconfig found. Try --disable-libconfig]))
    [EXTRA_LIBS="${EXTRA_LIBS} -lconfig" ; ]
  ])

AC_MSG_CHECKING([for liblua])
AC_ARG_ENABLE(liblua,[--enable-liblua/--disable-liblua],
  [ 
  if test "x$enableval" = "xno" ; then
    AC_MSG_RESULT([disabled])
  else
    AC_MSG_RESULT([enabled])
    AX_PROG_LUA([],[],
      [
        AX_LUA_HEADERS([],[AC_MSG_ERROR([No lua headers found. Try --disable-liblua])])
        AX_LUA_LIBS([],[AC_MSG_ERROR([No lua libs found. Try --disable-liblua])])
        [EXTRA_LIBS="${EXTRA_LIBS} ${LUA_LIB}" ; ]
        [CFLAGS="${CFLAGS} ${LUA_INCLUDE}" ; ]
        AC_DEFINE(USE_LUA,1,[use lua])
      ],
      [
        AC_MSG_ERROR([No lua found. Try --disable-liblua])
      ])
  fi 
  ],[
    AC_MSG_RESULT([enabled])
    AX_PROG_LUA([],[],
      [
        AX_LUA_HEADERS([],[AC_MSG_ERROR([No lua headers found. Try --disable-liblua])])
        AX_LUA_LIBS([],[AC_MSG_ERROR([No lua libs found. Try --disable-liblua])])
        [EXTRA_LIBS="${EXTRA_LIBS} ${LUA_LIB}" ; ]
        [CFLAGS="${CFLAGS} ${LUA_INCLUDE}" ; ]
        AC_DEFINE(USE_LUA,1,[use lua])
      ],
      [
        AC_MSG_ERROR([No lua found. Try --disable-liblua])
      ])
  ])
    
# Checks for header files.
AC_CHECK_HEADERS([fcntl.h malloc.h netdb.h stdlib.h string.h unistd.h arpa/inet.h mach/mach.h netinet/in.h sys/file.h sys/socket.h termios.h])

# Checks for typedefs, structures, and compiler characteristics.
AC_TYPE_SIZE_T
AC_TYPE_UID_T
AC_C_INLINE

# Checks for library functions.
AC_FUNC_MALLOC
AC_FUNC_REALLOC
AC_CHECK_FUNCS([alarm endpwent memset memmove mkdir select socket strdup strndup uname])

AC_SUBST(EXTRA_LIBS)
AC_CONFIG_FILES([Makefile])
AC_OUTPUT

