#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.
AC_INIT(tuxmath, 1.5.3, tuxmath-devel@lists.sourceforge.net)

#AC_PREREQ(2.59)
AC_CANONICAL_HOST
AC_CANONICAL_TARGET

AM_INIT_AUTOMAKE(tuxmath, 1.5.3)
AM_CONFIG_HEADER(config.h)
AM_MAINTAINER_MODE

NAME_VERSION=$PACKAGE-$VERSION

AC_DEFINE_UNQUOTED([PROGRAM_NAME], "$PACKAGE", [Name of the Program])

AC_SUBST(NAME_VERSION)

# Checks for programs.
AC_PROG_CC
AC_PROG_MAKE_SET

dnl Check for SDL 
SDL_VERSION=1.2.0

AM_PATH_SDL($SDL_VERSION, :,
         AC_MSG_ERROR([*** SDL version $SDL_VERSION not found!]) 
) 

CFLAGS="$CFLAGS $SDL_CFLAGS" 

# Check platform - see if WinMain needed:
AC_MSG_CHECKING([for native Win32])
case "$host" in
  *-*-mingw*)
    native_win32=yes
    ;;
  *)
    native_win32=no
    ;;
esac
AC_MSG_RESULT([$native_win32])

AM_CONDITIONAL(BUILD_MINGW32, test "$native_win32" = yes)
if test "$native_win32" = yes; then
   LIBS="$LIBS -lmingw32 -lSDLmain"
fi


LIBS="$LIBS $SDL_LIBS"

# Checks for libraries.
dnl Check for SDL_image: 
AC_CHECK_LIB([SDL_image], 
       [IMG_Load], 
       LIBS="$LIBS -lSDL_image", 
       [AC_MSG_ERROR([*** SDL_image library not found!])] 
)

dnl Check for SDL_mixer:. 
AC_CHECK_LIB([SDL_mixer], 
       [Mix_OpenAudio], 
       LIBS="$LIBS -lSDL_mixer", 
       [AC_MSG_ERROR([*** SDL_mixer library not found!])] 
)

dnl Check for SDL_ttf:
AC_CHECK_LIB([SDL_ttf],
	[TTF_Init],
	LIBS="$LIBS -lSDL_ttf",
	[AC_MSG_ERROR([SDL_ttf not found! http://www.libsdl.org/projects/SDL_ttf])]
)


# Checks for header files.
AC_HEADER_DIRENT
AC_HEADER_STDC
AC_CHECK_HEADERS([stdlib.h string.h unistd.h])

# Checks for typedefs, structures, and compiler characteristics.

# Checks for library functions.
AC_FUNC_CLOSEDIR_VOID
# AC_FUNC_MALLOC
AC_FUNC_STRTOD
AC_CHECK_FUNCS([getcwd memset mkdir strcasecmp strchr strndup])


dnl Set PACKAGE_DATA_DIR
PACKAGE_DATA_DIR="data"


AM_CONDITIONAL(BUILD_MINGW32, test "$native_win32" = yes)

if test "$native_win32" = yes; then
   AC_DEFINE([BUILD_MINGW32], 1,[Native MinGW32 build])
   CFLAGS="$CFLAGS -D__GW32__"
   LIBS="$LIBS -lgw32c -luuid -lole32 -lwsock32 -mwindows"
   CPPFLAGS="$CPPFLAGS -idirafter $prefix/include/glibc"
   #CPPFLAGS="$CPPFLAGS -I$prefix/include/glibc"
   AC_PATH_PROG(WINDRES, "$target_alias-windres", no, [$PATH])
fi

AC_SUBST(PACKAGE_DATA_DIR)


# Support for building NSIS Win32 installer:
NSI_INSTALL_DIR=mingw32
AC_SUBST(NSI_INSTALL_DIR)

NSI_DLL_DIR=~/tuxmath_dll
AC_ARG_WITH(dll-directory,AC_HELP_STRING(
    [--with-dll-directory=path],
    [set the path where dll for TuxMath are [$(NSI_DLL_DIR)]]), dll_path=$withval, dll_path=no)

AM_CONDITIONAL(NSI_BUILD, test "$dll_path" = no)

if test $dll_path != no; then
NSI_DLL_DIR=$dll_path
fi
AC_SUBST(NSI_DLL_DIR)

NSI_TUXMATH_CONF_DIR=no
AC_ARG_WITH(tuxmath_config-directory,AC_HELP_STRING(
    [--with-tuxmath_config-directory=path],
    [set the path where dll for TuxMath are [$(NSI_TUXMATH_CONF_DIR)]]), tuxmath_config_path=$withval, tuxmath_config_path=no)

AM_CONDITIONAL(NSI_USE_TUXMATH_CONF, test "$tuxmath_config_path" != no)

if test $tuxmath_config_path != no; then
NSI_TUXMATH_CONF_DIR=$tuxmath_config_path
fi

AC_SUBST(NSI_TUXMATH_CONF_DIR)

AC_PATH_PROG(NSIS, makensis, no, [$PATH])

AC_CONFIG_FILES([
Makefile
data/Makefile
data/fonts/Makefile
data/images/Makefile
data/images/backgrounds/Makefile
data/images/cities/Makefile
data/images/comets/Makefile
data/images/icons/Makefile
data/images/igloos/Makefile
data/images/penguins/Makefile
data/images/sprites/Makefile
data/images/status/Makefile
data/images/title/Makefile
data/images/tux/Makefile
data/missions/Makefile
data/missions/arcade/Makefile
data/missions/lessons/Makefile
data/sounds/Makefile
docs/Makefile
nsis/tuxmath.nsi
nsis/tuxmath_with_conf.nsi
src/Makefile
src/po/Makefile
])

AC_OUTPUT