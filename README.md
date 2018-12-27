# javacef-lab

## intro
JCEF or JAVA CEF is a CEF wrapper API for JAVA programming language.

## installation

If you get an error like this:
```
-- Could NOT find JNI (missing: JAVA_AWT_LIBRARY JAVA_JVM_LIBRARY JAVA_INCLUDE_PATH JAVA_INCLUDE_PATH2 JAVA_AWT_INCLUDE_PATH) (Required is at least version "1.7")
CMake Error at CMakeLists.txt:193 (message):...
```
Check then one or more of these files can not be found: https://cmake.org/cmake/help/v3.0/module/FindJNI.html
After downgrading to JAVA 8 I could finally find the file:
```
mindaugas@mindaugas-VirtualBox:/usr/lib/jvm$ find . -name "*jawt*"
./java-11-openjdk-amd64/lib/libjawt.so
./java-8-openjdk-amd64/jre/lib/amd64/libjawt.so
./java-8-openjdk-amd64/include/linux/jawt_md.h
./java-8-openjdk-amd64/include/jawt.h
./java-8-openjdk-amd64/lib/amd64/libjawt.so
```


## hello-world
