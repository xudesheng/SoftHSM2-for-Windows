# Building SoftHSM2 for Windows

This document describes process of building both 32-bit and 64-bit versions of [SoftHSM2](https://www.opendnssec.org/softhsm/) on 64-bit Windows 8.1 machine.

## Required software

- [Visual Studio](https://www.visualstudio.com/products/visual-studio-community-vs) (2015 Community)
- [GNU Privacy Guard for Windows](http://www.gpg4win.org/) (2.2.5)
- [7-zip](http://www.7-zip.org/) (9.20)
- [Strawberry Perl](http://strawberryperl.com/) (5.22.0.1)
- [The Netwide Assembler](http://www.nasm.us/) (2.11.08)
- [Python](https://www.python.org/) (3.5.1)
- [Notepad++](https://notepad-plus-plus.org/) (6.8.1)
- [GitHub Desktop](https://desktop.github.com/) (3.0.4.0)
- [WiX Toolset](http://wixtoolset.org/) (3.10.0.1823)

## Prepare working directories

    mkdir C:\build\bin\
    mkdir C:\build\src\

## Build OpenSSL 1.0.2g static library

Download [OpenSSL 1.0.2g](http://openssl.org/source/openssl-1.0.2g.tar.gz) with [its signature](http://openssl.org/source/openssl-1.0.2g.tar.gz.asc) into `C:\build\src\` directory and verify signature of the downloaded archive:

    cd C:\build\src\
    gpg --keyserver pgp.mit.edu --recv-keys 0E604491
    gpg --verify openssl-1.0.2g.tar.gz.asc openssl-1.0.2g.tar.gz

### 32-bit

Extract archive `openssl-1.0.2g.tar.gz` into `C:\build\src\openssl-1.0.2g-x86` directory:

    cd C:\build\src\
    "C:\Program Files\7-Zip\7z" x openssl-1.0.2g.tar.gz
    "C:\Program Files\7-Zip\7z" x openssl-1.0.2g.tar
    rename openssl-1.0.2g openssl-1.0.2g-x86
    del openssl-1.0.2g.tar

In a **new command line window** build OpenSSL and install it into `C:\build\bin\openssl-1.0.2g-x86` directory:

    cd C:\build\src\openssl-1.0.2g-x86
    set PATH=%PATH%;C:\nasm
    "C:\Program Files (x86)\Microsoft Visual Studio 14.0\VC\vcvarsall.bat"
    perl Configure VC-WIN32 --prefix=C:\build\bin\openssl-1.0.2g-x86 enable-static-engine
    ms\do_nasm
    nmake /f ms\nt.mak
    nmake /f ms\nt.mak test
    nmake /f ms\nt.mak install

## 64-bit

Extract archive `openssl-1.0.2g.tar.gz` into `C:\build\src\openssl-1.0.2g-x64` directory:

    cd C:\build\src\
    "C:\Program Files\7-Zip\7z" x openssl-1.0.2g.tar.gz
    "C:\Program Files\7-Zip\7z" x openssl-1.0.2g.tar
    rename openssl-1.0.2g openssl-1.0.2g-x64
    del openssl-1.0.2g.tar

In a **new command line window** build OpenSSL and install it into `C:\build\bin\openssl-1.0.2g-x64` directory:

    cd C:\build\src\openssl-1.0.2g-x64
    set PATH=%PATH%;C:\nasm
    "C:\Program Files (x86)\Microsoft Visual Studio 14.0\VC\vcvarsall.bat" amd64
    perl Configure VC-WIN64A --prefix=C:\build\bin\openssl-1.0.2g-x64 enable-static-engine
    ms\do_win64a
    nmake /f ms\nt.mak
    nmake /f ms\nt.mak test
    nmake /f ms\nt.mak install

## Build CppUnit 1.13.2 unicode library

Download [CppUnit 1.13.2](http://dev-www.libreoffice.org/src/cppunit-1.13.2.tar.gz) into `C:\build\src\` directory.

### 32-bit

Extract archive `cppunit-1.13.2.tar.gz` into `C:\build\src\cppunit-1.13.2-x86` directory:

    cd C:\build\src\
    "C:\Program Files\7-Zip\7z" x cppunit-1.13.2.tar.gz
    "C:\Program Files\7-Zip\7z" x cppunit-1.13.2.tar
    rename cppunit-1.13.2 cppunit-1.13.2-x86
    del cppunit-1.13.2.tar

Open solution `C:\build\src\cppunit-1.13.2-x86\src\CppUnitLibraries2010.sln` in Visual Studio and rebuild the source with `Release Unicode\Win32` solution configuration.

Copy the results into `C:\build\bin\cppunit-1.13.2-x86` directory:

    mkdir C:\build\bin\cppunit-1.13.2-x86\lib
    xcopy C:\build\src\cppunit-1.13.2-x86\lib C:\build\bin\cppunit-1.13.2-x86\lib /E
    mkdir C:\build\bin\cppunit-1.13.2-x86\include
    xcopy C:\build\src\cppunit-1.13.2-x86\include C:\build\bin\cppunit-1.13.2-x86\include /E

### 64-bit

Extract archive `cppunit-1.13.2.tar.gz` into `C:\build\src\cppunit-1.13.2-x64` directory:

    cd C:\build\src\
    "C:\Program Files\7-Zip\7z" x cppunit-1.13.2.tar.gz
    "C:\Program Files\7-Zip\7z" x cppunit-1.13.2.tar
    rename cppunit-1.13.2 cppunit-1.13.2-x64
    del cppunit-1.13.2.tar

Open solution `C:\build\src\cppunit-1.13.2-x64\src\CppUnitLibraries2010.sln` in Visual Studio and rebuild the source with `Release Unicode\x64` solution configuration.

Copy the results into `C:\build\bin\cppunit-1.13.2-x64` directory:

    mkdir C:\build\bin\cppunit-1.13.2-x64\lib
    xcopy C:\build\src\cppunit-1.13.2-x64\lib C:\build\bin\cppunit-1.13.2-x64\lib /E
    mkdir C:\build\bin\cppunit-1.13.2-x64\include
    xcopy C:\build\src\cppunit-1.13.2-x64\include C:\build\bin\cppunit-1.13.2-x64\include /E

## Build SoftHSM 2.1.0

Download [SoftHSM 2.1.0](https://dist.opendnssec.org/source/softhsm-2.1.0.tar.gz) with [its signature](https://dist.opendnssec.org/source/softhsm-2.1.0.tar.gz.sig) into `C:\build\src\` directory and verify signature of the downloaded archive:

    cd C:\build\src\
    gpg --keyserver pgp.mit.edu --recv-keys 4EE17CD2
    gpg --verify softhsm-2.1.0.tar.gz.sig softhsm-2.1.0.tar.gz

### 32-bit

Extract archive `softhsm-2.1.0.tar.gz` into `C:\build\src\softhsm-2.1.0-x86` directory:

    cd C:\build\src\
    "C:\Program Files\7-Zip\7z" x softhsm-2.1.0.tar.gz
    "C:\Program Files\7-Zip\7z" x softhsm-2.1.0.tar
    rename softhsm-2.1.0 softhsm-2.1.0-x86
    del softhsm-2.1.0.tar

Download [6b2588ba62bbe75465ee55caa28cdccf2e6d7b5b.patch](SoftHSM/6b2588ba62bbe75465ee55caa28cdccf2e6d7b5b.patch) and [19aa21262827d18d88adab73811e79f42c680743.patch](SoftHSM/19aa21262827d18d88adab73811e79f42c680743.patch) into `C:\build\src\` directory (see [opendnssec/SoftHSMv2#202](https://github.com/opendnssec/SoftHSMv2/pull/202) for more details). Apply these patches with the following commands executed in Git Shell (part of GitHub Desktop):

    cd C:\build\src\softhsm-2.1.0-x86\
    patch -p1 -i C:\build\src\6b2588ba62bbe75465ee55caa28cdccf2e6d7b5b.patch
    patch -p1 -i C:\build\src\19aa21262827d18d88adab73811e79f42c680743.patch

Configure build process in a **new command line window**:

    cd C:\build\src\softhsm-2.1.0-x86\win32\
    "C:\Program Files (x86)\Microsoft Visual Studio 14.0\VC\vcvarsall.bat"
    python Configure.py disable-debug with-crypto-backend=openssl with-openssl=C:\build\bin\openssl-1.0.2g-x86\ with-cppunit=C:\build\bin\cppunit-1.13.2-x86\

Open solution `C:\build\src\softhsm-2.1.0-x86\win32\softhsm2.sln` in Visual Studio and rebuild the source with `Release\Win32` solution configuration.

Verify the build by running the test programs:

    cd C:\build\src\softhsm-2.1.0-x86\win32\Release\
    cryptotest.exe
    datamgrtest.exe
    handlemgrtest.exe
    objstoretest.exe
    p11test.exe
    sessionmgrtest.exe
    slotmgrtest.exe

Copy the results into `C:\build\bin\softhsm-2.1.0-x86` directory:

    mkdir C:\build\bin\softhsm-2.1.0-x86
    copy C:\build\src\softhsm-2.1.0-x86\win32\Release\* C:\build\bin\softhsm-2.1.0-x86\

### 64-bit

Extract archive `softhsm-2.1.0.tar.gz` into `C:\build\src\softhsm-2.1.0-x64` directory:

    cd C:\build\src\
    "C:\Program Files\7-Zip\7z" x softhsm-2.1.0.tar.gz
    "C:\Program Files\7-Zip\7z" x softhsm-2.1.0.tar
    rename softhsm-2.1.0 softhsm-2.1.0-x64
    del softhsm-2.1.0.tar

Download [6b2588ba62bbe75465ee55caa28cdccf2e6d7b5b.patch](SoftHSM/6b2588ba62bbe75465ee55caa28cdccf2e6d7b5b.patch) and [19aa21262827d18d88adab73811e79f42c680743.patch](SoftHSM/19aa21262827d18d88adab73811e79f42c680743.patch) into `C:\build\src\` directory (see [opendnssec/SoftHSMv2#202](https://github.com/opendnssec/SoftHSMv2/pull/202) for more details). Apply these patches with the following commands executed in Git Shell (part of GitHub Desktop):

    cd C:\build\src\softhsm-2.1.0-x64\
    patch -p1 -i C:\build\src\6b2588ba62bbe75465ee55caa28cdccf2e6d7b5b.patch
    patch -p1 -i C:\build\src\19aa21262827d18d88adab73811e79f42c680743.patch

Configure build process in a **new command line window**:

    cd C:\build\src\softhsm-2.1.0-x64\win32\
    "C:\Program Files (x86)\Microsoft Visual Studio 14.0\VC\vcvarsall.bat" amd64
    python Configure.py enable-64bit disable-debug with-crypto-backend=openssl with-openssl=C:\build\bin\openssl-1.0.2g-x64\ with-cppunit=C:\build\bin\cppunit-1.13.2-x64\

Open solution `C:\build\src\softhsm-2.1.0-x64\win32\softhsm2.sln` in Visual Studio and rebuild the source with `Release\x64` solution configuration.

Verify the build by running the test programs:

    cd C:\build\src\softhsm-2.1.0-x64\win32\x64\Release\
    cryptotest.exe
    datamgrtest.exe
    handlemgrtest.exe
    objstoretest.exe
    p11test.exe
    sessionmgrtest.exe
    slotmgrtest.exe

Copy the results into `C:\build\bin\softhsm-2.1.0-x64` directory:

    mkdir C:\build\bin\softhsm-2.1.0-x64
    copy C:\build\src\softhsm-2.1.0-x64\win32\x64\Release\* C:\build\bin\softhsm-2.1.0-x64\

## Create portable archive

Create directory structure:

    mkdir C:\SoftHSM2\bin\
    mkdir C:\SoftHSM2\etc\
    mkdir C:\SoftHSM2\lib\
    mkdir C:\SoftHSM2\share\doc\openssl\
    mkdir C:\SoftHSM2\share\doc\softhsm2\
    mkdir C:\SoftHSM2\share\man\man1\
    mkdir C:\SoftHSM2\share\man\man5\
    mkdir C:\SoftHSM2\var\softhsm2\tokens\

Copy executables to `C:\SoftHSM2\bin\` directory:

    copy C:\build\bin\softhsm-2.1.0-x86\softhsm2-dump-file.exe C:\SoftHSM2\bin\
    copy C:\build\bin\softhsm-2.1.0-x86\softhsm2-keyconv.exe C:\SoftHSM2\bin\
    copy C:\build\bin\softhsm-2.1.0-x86\softhsm2-util.exe C:\SoftHSM2\bin\

Sign executables in `C:\SoftHSM2\bin\` directory in a **new command line window**:

    "C:\Program Files (x86)\Microsoft Visual Studio 14.0\VC\vcvarsall.bat"
    signtool.exe sign /sha1 84babdf3ba22669463db1ccfa7b1c917462bee4a /fd sha256 /tr http://time.certum.pl/ /td sha256 /d "SoftHSM2" /du "https://www.opendnssec.org/softhsm/" C:\SoftHSM2\bin\softhsm2-dump-file.exe
    signtool.exe sign /sha1 84babdf3ba22669463db1ccfa7b1c917462bee4a /fd sha256 /tr http://time.certum.pl/ /td sha256 /d "SoftHSM2" /du "https://www.opendnssec.org/softhsm/" C:\SoftHSM2\bin\softhsm2-keyconv.exe
    signtool.exe sign /sha1 84babdf3ba22669463db1ccfa7b1c917462bee4a /fd sha256 /tr http://time.certum.pl/ /td sha256 /d "SoftHSM2" /du "https://www.opendnssec.org/softhsm/" C:\SoftHSM2\bin\softhsm2-util.exe

Copy configuration file to `C:\SoftHSM2\etc\` directory:

    copy C:\build\bin\softhsm-2.1.0-x86\softhsm2.conf C:\SoftHSM2\etc\
    copy C:\SoftHSM2\etc\softhsm2.conf C:\SoftHSM2\etc\softhsm2.conf.sample

Copy PKCS#11 libraries to `C:\SoftHSM2\lib\` directory:

    copy C:\build\bin\softhsm-2.1.0-x86\softhsm2.dll C:\SoftHSM2\lib\softhsm2.dll
    copy C:\build\bin\softhsm-2.1.0-x64\softhsm2.dll C:\SoftHSM2\lib\softhsm2-x64.dll

Sign PKCS#11 libraries in `C:\SoftHSM2\lib\` directory in a **new command line window**:

    "C:\Program Files (x86)\Microsoft Visual Studio 14.0\VC\vcvarsall.bat"
    signtool.exe sign /sha1 84babdf3ba22669463db1ccfa7b1c917462bee4a /fd sha256 /tr http://time.certum.pl/ /td sha256 /d "SoftHSM2" /du "https://www.opendnssec.org/softhsm/" C:\SoftHSM2\lib\softhsm2.dll
    signtool.exe sign /sha1 84babdf3ba22669463db1ccfa7b1c917462bee4a /fd sha256 /tr http://time.certum.pl/ /td sha256 /d "SoftHSM2" /du "https://www.opendnssec.org/softhsm/" C:\SoftHSM2\lib\softhsm2-x64.dll

Copy licenses to `C:\SoftHSM2\share\doc\` directory:

    copy C:\build\src\openssl-1.0.2g-x64\LICENSE C:\SoftHSM2\share\doc\openssl\LICENSE.txt
    copy C:\build\src\softhsm-2.1.0-x64\LICENSE C:\SoftHSM2\share\doc\softhsm2\LICENSE.txt

Copy manual pages to `C:\SoftHSM2\share\man\` directory:

    copy C:\build\src\softhsm-2.1.0-x64\src\lib\common\softhsm2.conf.5.in C:\SoftHSM2\share\man\man5\
    copy C:\build\src\softhsm-2.1.0-x64\src\bin\dump\softhsm2-dump-file.1 C:\SoftHSM2\share\man\man1\
    copy C:\build\src\softhsm-2.1.0-x64\src\bin\keyconv\softhsm2-keyconv.1 C:\SoftHSM2\share\man\man1\
    copy C:\build\src\softhsm-2.1.0-x64\src\bin\util\softhsm2-util.1 C:\SoftHSM2\share\man\man1\

Move manual pages from `C:\SoftHSM2\share\man\` to Linux box and convert them to text files:

    export MANWIDTH=80
    man ./softhsm2.conf.5.in | col -b > ./softhsm2.conf.txt
    man ./softhsm2-dump-file.1 | col -b > ./softhsm2-dump-file.txt
    man ./softhsm2-keyconv.1 | col -b > ./softhsm2-keyconv.txt
    man ./softhsm2-util.1 | col -b > ./softhsm2-util.txt

Move converted text files from Linux box back to `C:\SoftHSM2\share\man\` directory.

Convert EOL of all `txt` files in `C:\SoftHSM2\share\` directory to Windows format with Notepad++.

Update file `C:\SoftHSM2\share\man\man5\softhsm2.conf.txt` with [Windows specific information](https://github.com/disig/SoftHSM2-for-Windows/commit/1cce5cdcee7fdbab96549bfa0c8fab6081ff1aec).

Convert EOL of `C:\SoftHSM2\etc\softhsm2.conf` and `C:\SoftHSM2\etc\softhsm2.conf.sample` files to Windows format with Notepad++ and update `C:\SoftHSM2\etc\softhsm2.conf` file with [Windows specific information](https://github.com/disig/SoftHSM2-for-Windows/commit/2e26ee77387a34c4be6b6646baad9e2097107879).

Create file [`C:\SoftHSM2\README.txt`](SoftHSM/misc/README.txt).

Create archive `C:\build\SoftHSM2-2.1.0-portable.zip`:

    cd C:\build
    "C:\Program Files\7-Zip\7z" a SoftHSM2-2.1.0-portable.zip C:\SoftHSM2

## Create MSI installer

Clone `SoftHSM2-for-Windows` repository in Git Shell:

    cd C:\build\src\
    git clone https://github.com/disig/SoftHSM2-for-Windows/

Open solution `C:\build\src\SoftHSM2-for-Windows\WiX\SoftHSM2.sln` in Visual Studio and rebuild the source with `Release\x86` solution configuration.

Copy generated MSI installer to `C:\build\` directory:

    copy C:\build\src\SoftHSM2-for-Windows\WiX\SoftHSM2\bin\Release\SoftHSM2.msi C:\build\SoftHSM2-2.1.0.msi

Sign MSI installer in a **new command line window**:

    "C:\Program Files (x86)\Microsoft Visual Studio 14.0\VC\vcvarsall.bat"
    signtool.exe sign /sha1 84babdf3ba22669463db1ccfa7b1c917462bee4a /fd sha256 /tr http://time.certum.pl/ /td sha256 /d "SoftHSM2" /du "https://www.opendnssec.org/softhsm/" C:\build\SoftHSM2-2.1.0.msi
