# javacef-lab

## intro
JCEF or JAVA CEF is a CEF wrapper API for JAVA programming language.

## installation

General instruction: https://bitbucket.org/chromiumembedded/java-cef/wiki/BranchesAndBuilding

Additional remarks:
- If you get an error like this:
   ```
   Could NOT find JNI (missing: JAVA_INCLUDE_PATH JAVA_INCLUDE_PATH2 JAVA_AWT_INCLUDE_PATH)...
   ```
- check then one or more of these files can not be found: https://cmake.org/cmake/help/v3.0/module/FindJNI.html
- after downgrading to JAVA 8 I could finally find the file:
   ```mindaugas@mindaugas-VirtualBox:/usr/lib/jvm$ find . -name "*jawt*"
   ./java-11-openjdk-amd64/lib/libjawt.so
   ./java-8-openjdk-amd64/jre/lib/amd64/libjawt.so
   ./java-8-openjdk-amd64/include/linux/jawt_md.h
   ./java-8-openjdk-amd64/include/jawt.h
   ./java-8-openjdk-amd64/lib/amd64/libjawt.so
   ```
- Delete all of the files in `jcef_build` directory
- Retry cmake: `cmake -G "Unix Makefiles" -DCMAKE_BUILD_TYPE=Release ..`
- proceed with `make -j<core_count>`

Successfull build process should end with this console output:
```
[ 97%] Building CXX object native/CMakeFiles/jcef.dir/critical_wait_posix.cpp.o
[ 98%] Building CXX object native/CMakeFiles/jcef.dir/jni_util_linux.cpp.o
[ 98%] Building CXX object native/CMakeFiles/jcef.dir/signal_restore_posix.cpp.o
[ 99%] Building CXX object native/CMakeFiles/jcef.dir/temp_window_x11.cc.o
[ 99%] Building CXX object native/CMakeFiles/jcef.dir/util_linux.cpp.o
[ 99%] Building CXX object native/CMakeFiles/jcef.dir/util_posix.cpp.o
[100%] Linking CXX shared library Release/libjcef.so

*** Run the following commands manually to create necessary symlinks ***
sudo ln -s /home/mindaugas/java-cef/third_party/cef/cef_binary_3.3538.1852.gcb937fc_linux64/Resources/icudtl.dat /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/icudtl.dat
sudo ln -s /home/mindaugas/java-cef/third_party/cef/cef_binary_3.3538.1852.gcb937fc_linux64/Release/natives_blob.bin /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/natives_blob.bin
sudo ln -s /home/mindaugas/java-cef/third_party/cef/cef_binary_3.3538.1852.gcb937fc_linux64/Release/snapshot_blob.bin /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/snapshot_blob.bin
sudo ln -s /home/mindaugas/java-cef/third_party/cef/cef_binary_3.3538.1852.gcb937fc_linux64/Release/v8_context_snapshot.bin /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/v8_context_snapshot.bin

[100%] Built target jcef
```

## hello-world
