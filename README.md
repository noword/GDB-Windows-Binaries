# gdb Windows binaries

The source of gdb was cloned from git://sourceware.org/git/binutils-gdb.git.

Compiled with mingw-w64 v12.2.0, includes all architectures, TUI supported, python supported, without any other dll dependencies except Windows own.

### build config

```bash
# /bin/bash

PYTHON_PATH=/c/Python38

export CFLAGS="-Os -s -ffunction-sections -fdata-sections -static-libgcc -static  -static-libstdc++ -DNCURSES_STATIC -DNCURSES_VERSION"
export CPPFLAGS="$CFLAGS"

if ! [ -z %PYTHON_PATH ]; then
    export CPPFLAGS="$CFLAGS -DPy_BUILD_CORE_BUILTIN=1" 
    export PYCONFIG="--with-python=$PYTHON_PATH"
fi

export CXXFLAGS="$CPPFLAGS"
export LDFLAGS="-Wl,--gc-sections"

../configure \
--host=x86_64-w64-mingw32 \
--enable-targets=all \
--disable-nls \
--disable-sim \
--disable-gas \
--disable-binutils \
--disable-ld \
--disable-gprof \
--disable-werror \
--enable-build-warnings=no \
--disable-rpath \
--without-guile \
--without-babeltrace \
--without-libunwind-ia64 \
--enable-tui \
--enable-gdbserver=yes \
--with-expat \
--without-auto-load-safe-path \
CC="ccache x86_64-w64-mingw32-gcc" \
CXX="ccache x86_64-w64-mingw32-g++" \
RANLIB="ccache x86_64-w64-mingw32-gcc-ranlib" \
$PYCONFIG

```