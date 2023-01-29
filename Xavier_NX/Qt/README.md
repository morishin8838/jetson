# Qt5.12.8 setup for NX

## ターゲットデバイスNX Qtインストール手順
* NX上で作業する。
1. Qt apt install
```
$sudo apt install -y build-essential mesa-common-dev
$sudo apt install -y libglu1-mesa-dev mesa-common-dev freeglut3 libglfw3 freeglut3-dev libglew1.5-dev at-spi2-core libopencv-dev python3-opencv qtbase5-dev qttools5-dev-tools qt5-default qtdeclarative5-dev qtbase5-private-dev 
$sudo apt install -y qtdeclarative5-private-dev libqt5opengl5-dev qml-module-qtquick-controls qml-module-qt-labs-platform qml-module-qtquick-controls2 qml
$sudo apt install -y qml-module-qtmultimedia qtquickcontrols2-5-dev qml-module-qtquick2 qml-module-qt-labs-folderlistmodel qml-module-qtquick-extras  libqt5quickcontrols2-5 qtcreator qtcreator-doc libqt5serialport5-dev qtconnectivity5-dev qtmultimedia5-dev qtquickcontrols2-5-dev
```

## ホストクロスコンパイル環境構築手順
* ホスト環境で作業する。
* バージョンは5.12.8であることを確認する
* バージョンは5.12.8の場合、KRIAと同じQTバージョンを使えます

1. rootfs取得 デバイスデータ同期 
```
$mkdr ~/Nvidia/XavierNX
$cd XavierNX
$mkdir -p rootfs/{lib,sbin,usr/{include,lib,bin},etc/alternatives}            
$sudo rsync -e ssh -avz a12348838@192.168.2.99:/lib rootfs/
$sudo rsync -e ssh -avz a12348838@192.168.2.99:/sbin rootfs/
$sudo rsync -e ssh -avz a12348838@192.168.2.99:/usr/include rootfs/usr
$sudo rsync -e ssh -avz --copy-links --exclude build a12348838@192.168.2.99:/usr/lib rootfs/usr
#$sudo rsync -e ssh -avz a12348838@192.168.2.99:/usr/bin rootfs/usr
$sudo rsync -e ssh -avz a12348838@192.168.2.99:/etc/alternatives rootfs/etc       
$sudo rsync -e ssh -avz a12348838@192.168.2.99:/usr/include rootfs/usr
```

2．ターゲットデバイスのコンパイラバージョンを確認する
```
$ssh a12348838@192.168.2.99
$g++ --version
g++ (Ubuntu 9.4.0-1ubuntu1~20.04.1) 9.4.0
$exit
```

2. Qt ダウンロードと解凍
```
$wget https://download.qt.io/archive/qt/5.12/5.12.8/single/qt-everywhere-opensource-src-5.12.8.tar.xz
$sudo chmod 775 qt-everywhere-opensource-src-5.12.8.tar.xz
$xz -dc qt-everywhere-opensource-src-5.12.8.tar.xz | tar xfv -
```

3. ホスト側にコピーしたシンボリックリンクをホスト向けに修正
```
$wget https://github.com/alpqr/fixsymlinks_tx1
$cd $HOME/Nvidia/XavierNX
$sudo ./fixsymlinks.sh ./rootfs
```

4. コンフィグ
```
$cd qt-everywhere-src-5.12.8/
$mkdir build
$cd build
$ROOT_DIR=$HOME/Nvidia/XavierNX
$ROOTFS=$ROOT_DIR/rootfs
../configure -v -release -opensource -confirm-license -xplatform linux-aarch64-gnu-g++ -device-option CROSS_COMPILE=aarch64-linux-gnu- -no-pch -opengl es2 -no-eglfs -no-openssl -no-qml-debug -nomake tools -nomake examples -nomake tests -prefix /usr/local/qt5.12.8 -extprefix $ROOT_DIR/qt5.12.8 -hostprefix $ROOT_DIR/qt5.12.8-host -sysroot $ROOTFS QMAKE_CFLAGS_ISYSTEM=
```

5. ビルド

```
$make clean || make -j8
```

6. Qt Creator設定
* ビルドしたフォルダqt5.12.8-host/bin/qmakeを用いて、JetsonNX用のQtバージョンを作成する。


