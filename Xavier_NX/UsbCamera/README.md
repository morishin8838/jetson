# Qt5.15.8 setup for NX


## 手順

1. Qt ダウンロードと解凍
```
$cd ~/Downloads
$wget https://download.qt.io/archive/qt/5.15/5.15.8/single/qt-everywhere-opensource-src-5.15.8.tar.xz
$tar qt-everywhere-opensource-src-5.15.8.tar.xz
```

2. ビルド 
```
$cd ./qt-everywhere-src-5.15.8
$mkdir build
$cd build
$../configure -v -release -opensource -confirm-license -xplatform linux-aarch64-gnu-g++ -device-option CROSS_COMPILE=aarch64-linux-gnu- -no-pch -opengl es2 -no-eglfs -no>
$make clean || make -j8 || sudo make install
$sudo ln -sf /usr/local/Qt-5.15.8/bin/qmake /usr/lib/aarch64-linux-gnu/qt5/qmake
```



