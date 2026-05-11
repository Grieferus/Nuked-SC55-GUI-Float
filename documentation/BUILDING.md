# Building

This is a building guide for newbies like me.

### Windows
For building on Windows, I recommend MinGW64 which is part of MSYS2.

Full building process goes like this:

- Open MinGW64;
- Install necessary dependencies like this:
```bash
pacman -S make git gettext base-devel libtool pkg-config mingw-w64-x86_64-cmake mingw-w64-x86_64-SDL2 mingw-w64-x86_64-toolchain mingw-w64-x86_64-gcc-libs
```
- Clone the repository and go to the folder:
```bash
git clone https://github.com/Grieferus/Nuked-SC55-GUI-Float
cd Nuked-SC55-GUI-Float
```
- Create a build folder and go to it:
```bash
mkdir build
cd build
```
- Create an `app.rc` file in `/src` folder and define the path like this:
```rc
IDI_ICON1 ICON "Nuked-SC55-GUI-Float/data/nuked-icon-ico.ico"
```
- Go to `CMakeFiles.txt` and add `app.rc` to the source list of standard frontend:
```cmake
PRIVATE
...
src/app.rc
```
This will make sure that the `nuked-sc55` will have an icon.

- Use CMake to generate a solution:
```bash
cmake -DCMAKE_BUILD_TYPE=Release ..
cmake --build .
```
This will generate `nuked-sc55-backend` library file and two binaries `nuked-sc55` and `nuked-sc55-render`. Copy these three into your Nuked-SC55 folder.

#### If your path contains non-ASCII characters, do this:
- Create a folder in C:/, either manually or in MinGW64. In MinGW64, it goes like this:
```bash
cd /c
mkdir projects
cd projects
```
- Clone the repository;
- in `app.rc` define the full path like this:
```rc
IDI_ICON1 ICON "C:/projects/Nuked-SC55-GUI-Float/data/nuked-icon-ico.ico"
```
- Perform the rest of the steps as usual.
This is a problem of MSYS2 as a whole which has to do with its UNIX heritage.

If you're building a binary to only run on your local machine, consider adding `-DCMAKE_CXX_FLAGS="-march=native -mtune=native" -DCMAKE_INTERPROCEDURAL_OPTIMIZATION=ON` to the first cmake command to enable more optimizations.
`-DBUILD_SHARED_LIBS=OFF -DCMAKE_EXE_LINKER_FLAGS="-static"` arguments can be added for static linking.

### ASIO (Optional)

To enable ASIO support, pass `-DNUKED_ENABLE_ASIO=ON` and `-DNUKED_ASIO_SDK_DIR=<path>` where `<path>` points to the extracted ASIO SDK obtained from [here](https://www.steinberg.net/developers/).

If you want a static linking, you can add `-DCMAKE_EXE_LINKER_FLAGS="-static"` flag as well.

## Mac and Linux

For building on Mac and Linux, you need rtmidi

Full building process is similar to the Windows one, but I will still add it here:
```bash
git clone git@github.com:Grieferus/Nuked-SC55-GUI-Float.git
cd Nuked-SC55-GUI-Float
mkdir build
cd build
cmake -DCMAKE_BUILD_TYPE=Release -DUSE_RTMIDI=ON ..
cmake --build .
```
