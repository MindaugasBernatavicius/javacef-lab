# javacef-lab

## intro
JCEF or JAVA CEF is a CEF wrapper API for JAVA programming language.

## installation

GEneral instruction: https://bitbucket.org/chromiumembedded/java-cef/wiki/BranchesAndBuilding

If you get an error like this:
```-- Could NOT find JNI (missing: JAVA_INCLUDE_PATH JAVA_INCLUDE_PATH2 JAVA_AWT_INCLUDE_PATH) (Required is at least version "1.7")```
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

## hello-world
