# android-openssl-qt
scripts to generate openssl .so to be used from Qt Android Projects on MacOS

Building Qt mobile Apps for Android there's a problem if App should run on Android 7+, because Google removed openssl.
You must build openssl .so libs by yourself

There's a documentation from Qt HowTo add openssl:
http://doc.qt.io/qt-5/opensslsupport.html 

Unfortunately this fails on MacOS with error 
unknown argument: '-mandroid'

see also QTBUG-59375  https://bugreports.qt.io/browse/QTBUG-59375 

thx Marco Piccolino and Roman Pasechnik
in this repo I found the needed scripts to generate the openssl libraries by myself.
https://github.com/orangefour/android-openssl 

These scripts do all for you from downloading and extracting openssl and generating libs for x86 and armeabi-v7a

I'm not providing any prebuilt .so here as you may found in other repos at github,
because this would be a security hole to embed .so you don't have built from origin sources.

I did small modifications to the scripts to run them on OSX
Please check the environment values of setenv-android-mod.sh

## HowTo create libssl.so and libcrypto.so
Clone this repo to a location you can refer to from your Qt projects.

Open Terminal and do:

```
cd <path/to/this/repo>
chmod 755 ./build-all-arch.sh
chmod 755 ./setenv-android-mod.sh
export ANDROID_NDK_ROOT=/your/path/to/_android/android-ndk-r10e
export OPENSSL_VERSION="openssl-1.0.2k"
./build-all-arch.sh
```

## Modify .pro
insert this line into your .pro

```
include(android-openssl.pri)
```

## copy android-openssl.pri
copy this .pri into your projects (don't forget to adjust the path)

```
android {
  ANDROID_EXTRA_LIBS += $$PWD/my/path/to/prebuilt/armeabi-v7a/libcrypto.so
  ANDROID_EXTRA_LIBS += $$PWD/my/path/to/android-openssl/prebuilt/armeabi-v7a/libssl.so
}
```
