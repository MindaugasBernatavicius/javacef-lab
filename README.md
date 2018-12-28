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

While testing the installation I received the error:
```
mindaugas@mindaugas-VirtualBox:~/java-cef/tools$ ./run.sh linux64 Release simple
Exception in thread "main" java.lang.UnsatisfiedLinkError: /home/mindaugas/java-cef/jcef_build/native/Release/libjcef.so: libjawt.so: cannot open shared object file: No such file or directory
	at java.base/java.lang.ClassLoader$NativeLibrary.load0(Native Method)
	at java.base/java.lang.ClassLoader$NativeLibrary.load(ClassLoader.java:2424)
	at java.base/java.lang.ClassLoader$NativeLibrary.loadLibrary(ClassLoader.java:2481)
	at java.base/java.lang.ClassLoader.loadLibrary0(ClassLoader.java:2678)
	at java.base/java.lang.ClassLoader.loadLibrary(ClassLoader.java:2643)
	at java.base/java.lang.Runtime.loadLibrary0(Runtime.java:876)
	at java.base/java.lang.System.loadLibrary(System.java:1875)
	at org.cef.CefApp.startup(CefApp.java:518)
	at tests.simple.MainFrame.main(MainFrame.java:140)
```

Even though CEF build succeeded with seemingly correct options:
```-- *** JCEF CONFIGURATION SETTINGS ***
-- Python executable:            /usr/bin/python
-- Java directory:               /usr/lib/jvm/java-8-openjdk-amd64
-- JNI libraries:                /usr/lib/jvm/java-8-openjdk-amd64/jre/lib/amd64/libjawt.so;/usr/lib/jvm/java-8-openjdk-amd64/jre/lib/amd64/server/libjvm.so
-- JNI include directories:      /usr/lib/jvm/java-8-openjdk-amd64/include;/usr/lib/jvm/java-8-openjdk-amd64/include/linux;/usr/lib/jvm/java-8-openjdk-amd64/include
```

- My initial idea is that I still have Java 10 as my main Java installation (so javac and java commands are v10, not v8) and scripts like "compile.sh" use that installation. Need to change that:
   ```
   mindaugas@mindaugas-VirtualBox:~/java-cef/tools$ ls -laht /usr/bin/javac
   lrwxrwxrwx 1 root root 23 lapkr 29 01:06 /usr/bin/javac -> /etc/alternatives/javac
   ````
   ... so I updated the laternatives for java and javac: `sudo update-alternatives --config javac`. Go the same error, but solved a warning: 
   ```
   mindaugas@mindaugas-VirtualBox:~/java-cef/tools$ ./compile.sh linux64
   Note: Some input files use or override a deprecated API.
   Note: Recompile with -Xlint:deprecation for details.

   ```
- What helped was exporting the java-8 jre/lib/amd64 folder to LD_LIBRARY_PATH:
	```vim run.sh
	export LD_LIBRARY_PATH=$LIB_PATH:/usr/lib/jvm/java-8-openjdk-amd64/jre/lib/amd64
	```

## hello-world
