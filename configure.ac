# DYMO LabelWriter Drivers
# Copyright (C) 2008 Sanford L.P.

# This program is free software; you can redistribute it and/or
# modify it under the terms of the GNU General Public License
# as published by the Free Software Foundation; either version 2
# of the License, or (at your option) any later version.

# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.

# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software
# Foundation, Inc., 59 Temple Place - Suite 330, Boston, MA  02111-1307, USA.

AC_PREREQ(2.59)
AC_INIT(dymo-cups-drivers, 2.0.0.0, philipp.ineichen@dymo.com)
AM_INIT_AUTOMAKE([subdir-objects])
AC_CONFIG_SRCDIR([src/lw/LabelWriterDriver.h])
AC_CONFIG_HEADER([src/config.h])

# get cups dirs
# if there is cups-config utility - use it
# otherwise if there are user supplyed values - use it
# otherwise - use default values
cupsdatadir=$BUILDROOT$cups_datadir
cupsserverbindir=$BUILDROOT$cups_serverbindir
cups_cflags=""

if test x"$cupsdatadir" = x; then
    cupsdatadir=$BUILDROOT/usr/share/cups
fi

if test x"$cupsserverbindir" = x; then    
    cupsserverbindir=$BUILDROOT/usr/lib/cups
fi    

#check for cups-config
cupsc=""
AC_PATH_PROG(cupsc, cups-config)
if test x"$cupsc" != x; then
    cupsdatadir=$BUILDROOT`cups-config --datadir`
    cupsserverbindir=$BUILDROOT`cups-config --serverbin`
    cups_cflags=`cups-config --cflags`
fi 

cupsmodeldir=${cupsdatadir}/model
cupsfilterdir=${cupsserverbindir}/filter

AC_SUBST(cupsmodeldir)
AC_SUBST(cupsfilterdir)

AC_ARG_ENABLE([samples],
[  --enable-samples    Build with samples],
[case "${enableval}" in
  yes) samples=true ;;
  no)  samples=false ;;
  *) AC_MSG_ERROR([bad value ${enableval} for --enable-samples]) ;;
esac],
[samples=false])

AM_CONDITIONAL([SAMPLES], [test x$samples = xtrue])

#samplesdir=samples
#AC_SUBST(samplesdir)

# check for proper CUPS version - 1.1 for now
#cups_version=`cups-config --api-version`
#if test x"$cups_version" = x1.0; then
#    AC_ERROR(CUPS version 1.1 is required)
#fi 

AC_SUBST(cups_cflags)

warning_flags="-Wall -Wno-unknown-pragmas"
debug_flags=""
optimization_flags="-O2"
CXXFLAGS="${optimization_flags} ${warning_flags} ${debug_flags} ${cups_cflags}"

AC_CHECK_LIB(cups, cupsMarkOptions, , AC_ERROR([Can't find cups library]))
AC_CHECK_LIB(cupsimage, cupsRasterReadHeader, , AC_ERROR([Can't find cupsimage library]))
AC_CHECK_HEADERS([cups/cups.h cups/raster.h],, AC_ERROR([Can't find cups headers]))

AC_CHECK_LIB(pthread, pthread_create,, AC_ERROR([Can't find pthread library]))
AC_CHECK_HEADERS([pthread.h],, AC_ERROR([Can't find pthread headers]))

AC_SUBST(CUPS_LIBS)

# Checks for programs.
AC_PROG_CXX
AC_PROG_CC

# Checks for libraries.

# Checks for header files.
AC_HEADER_STDC
AC_CHECK_HEADERS([fcntl.h stdlib.h string.h unistd.h])

# Checks for typedefs, structures, and compiler characteristics.
AC_HEADER_STDBOOL
AC_C_CONST
AC_C_INLINE
AC_TYPE_SIZE_T

# Checks for library functions.
AC_CHECK_FUNCS([memset strcasecmp])

AC_CONFIG_FILES([Makefile src/Makefile src/common/tests/Makefile src/lm/Makefile src/lm/tests/Makefile src/lw/Makefile src/lw/tests/Makefile ppd/Makefile docs/Makefile samples/Makefile samples/paper_list/Makefile samples/paper_bounds/Makefile samples/test_label/Makefile samples/custom_paper/Makefile samples/custom_paper_tape/Makefile])
AC_OUTPUT



