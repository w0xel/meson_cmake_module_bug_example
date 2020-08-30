# Meson CMake Module Bug Example

This repository shows a minimal example showing off a CMake module bug in meson.

Related bug report: https://github.com/mesonbuild/meson/issues/7666

## Execute

As usual:

```{.sh}
meson build
ninja -Cbuild
```

## Expected

Meson creates directories not found but used as include_directories in CMake, if they are tin the CMake build direcotory within the meson build directory.

## Error as of meson 0.55.1, CMake 3.18.1

During configuration:
```
|CMake configuration: SUCCEEDED
|WARNING: CMake: path <redacted>/meson_cmake_bug_example/build/subprojects/example_subproject/__CMake_build/somewhere does not exist.
|WARNING:  --> Ignoring. This can lead to build errors.
|CMake project example_subproject has 2 build targets.
```

During build:
```
[2/4] Compiling C object subprojects/example_subproject/libcm_main.a.p/main.c.o
FAILED: subprojects/example_subproject/libcm_main.a.p/main.c.o 
cc -Isubprojects/example_subproject/libcm_main.a.p -Isubprojects/example_subproject -I../subprojects/example_subproject -I../../subprojects/example_subproject -fdiagnostics-color=always -pipe -D_FILE_OFFSET_BITS=64 -Wall -Winvalid-pch -g -MD -MQ subprojects/example_subproject/libcm_main.a.p/main.c.o -MF subprojects/example_subproject/libcm_main.a.p/main.c.o.d -o subprojects/example_subproject/libcm_main.a.p/main.c.o -c ../subprojects/example_subproject/main.c
../subprojects/example_subproject/main.c:1:10: fatal error: gen/main.h: No such file or directory
    1 | #include "gen/main.h"
      |          ^~~~~~~~~~~~
compilation terminated.
```

Creating the missing directory and issuing `meson build --reconfigure` is enough to make it compile.

### Explanation

Meson does not find specific directories of generated headers, and ignores them. Instead, meson should generate the directories