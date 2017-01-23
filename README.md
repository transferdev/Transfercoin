<p align="center">
  <img src="https://github.com/empinel/Ion-Core/blob/master/src/qt/res/images/splash.png" alt="Ion_Icon"/>
</p>
# **Ion-Core (ION) v1.0**

[![Build Status](https://travis-ci.org/empinel/Ion-Core.png?branch=master)](https://travis-ci.org/empinel/Ion-Core)

Ion Integration/Staging Tree
================================

**Copyright (c) 2015-2017 Empinel**

#### What is Ion?
----------------
* Coin Suffix: ION
* PoW Algorithm: SHA256d
* PoW Period: 2,000 Network Initiation Blocks
* PoW Median Target Spacing: 60 Seconds 
* PoW Difficulty Retarget: 2 Blocks 
* Full Confirmation: 10 Blocks
* PoS Algorithm: SHA256d
* PoS Target Spacing: 64 Seconds 
* PoS Difficulty Retarget: 2 Blocks 
* PoS Reward: Varied
* PoS Min: 1 Day
* PoS Max: Unlimited
* Total Coins: 20,000,000 ION
* Block Size: 8 Mega-bytes (MB)


Ion is a digital currency that enables instant payments to anyone, anywhere in the world. Ion uses peer-to-peer technology over ClearNet to operate with no central authority (centralisation): managing transactions and issuing currency (ION) are carried out collectively by the Ion network. Ion is the name of open source software which enables the use of the currency ION.



**MainNet Parameters**
P2P Port = 58273
RPC Port = 59273


**TestNet Parameters**
P2P Port = 27170
RPC Port = 27171


UNIX BUILD NOTES
====================
Some notes on how to build Ion in Unix. 

Note
---------------------
Always use absolute paths to configure and compile Ion and the dependencies,
for example, when specifying the the path of the dependency:

    ../dist/configure --enable-cxx --disable-shared --with-pic --prefix=$BDB_PREFIX

Here BDB_PREFIX must absolute path - it is defined using $(pwd) which ensures
the usage of the absolute path.

To Build
---------------------

```bash
qmake
make
make install # optional
```

This will build ion-qt as well if the dependencies are met.

Dependencies
---------------------

These dependencies are required:

 Library     | Purpose          | Description
 ------------|------------------|----------------------
 libssl      | SSL Support      | Secure communications
 libboost    | Boost            | C++ Library

Optional dependencies:

 Library     | Purpose          | Description
 ------------|------------------|----------------------
 miniupnpc   | UPnP Support     | Firewall-jumping support
 libdb4.8    | Berkeley DB      | Wallet storage (only needed when wallet enabled)
 qt          | GUI              | GUI toolkit (only needed when GUI enabled)
 protobuf    | Payments in GUI  | Data interchange format used for payment protocol (only needed when GUI enabled)
 libqrencode | QR codes in GUI  | Optional for generating QR codes (only needed when GUI enabled)

For the versions used in the release, see [release-process.md](release-process.md) under *Fetch and build inputs*.

System requirements
--------------------

C++ compilers are memory-hungry. It is recommended to have at least 1 GB of
memory available when compiling Ion Core. With 512MB of memory or less
compilation will take much longer due to swap thrashing.

Dependency Build Instructions: Ubuntu & Debian
----------------------------------------------
Build requirements:

    sudo apt-get install git build-essential libtool autotools-dev autoconf pkg-config libssl-dev libcrypto++-dev libevent-dev libminiupnpc-dev libgmp-dev
    
for Ubuntu 12.04 and later or Debian 7 and later libboost-all-dev has to be installed:

    sudo apt-get install libboost-all-dev

 db4.8 packages are available [here](https://launchpad.net/~silknetwork/+archive/ubuntu/silknetwork).
 You can add the repository using the following command:

        sudo add-apt-repository ppa:silknetwork/silknetwork
        sudo apt-get update

 Ubuntu 12.04 and later have packages for libdb5.1-dev and libdb5.1++-dev,
 but using these will break binary wallet compatibility, and is not recommended.

for Debian 7 (Wheezy) and later:
 The oldstable repository contains db4.8 packages.
 Add the following line to /etc/apt/sources.list,
 replacing [mirror] with any official debian mirror.

    deb http://[mirror]/debian/ oldstable main

To enable the change run

    sudo apt-get update

for other Debian & Ubuntu (with ppa):

    sudo apt-get install libdb4.8-dev libdb4.8++-dev

Optional:

    sudo apt-get install libminiupnpc-dev (see --with-miniupnpc and --enable-upnp-default)

Dependencies for the GUI: Ubuntu & Debian
-----------------------------------------

If you want to build Ion-Qt, make sure that the required packages for Qt development
are installed. Qt 5 is necessary to build the GUI.
If both Qt 4 and Qt 5 are installed, Qt 5 will be used. Pass `--with-gui=qt5` to configure to choose Qt5.
To build without GUI pass `--without-gui`.

For Qt 5 you need the following:

    sudo apt-get install libqt5gui5 libqt5core5a libqt5dbus5 qttools5-dev qttools5-dev-tools libprotobuf-dev protobuf-compiler libcrypto++-dev

libqrencode (optional) can be installed with:

    sudo apt-get install libqrencode-dev

Once these are installed, they will be found by configure and a ion-qt executable will be
built by default.

Notes
-----
The release is built with GCC and then "strip xiond" to strip the debug
symbols, which reduces the executable size by about 90%.


miniupnpc
---------

[miniupnpc](http://miniupnp.free.fr/) may be used for UPnP port mapping.  It can be downloaded from [here](
http://miniupnp.tuxfamily.org/files/).  UPnP support is compiled in and
turned off by default.  See the configure options for upnp behavior desired:

    --without-miniupnpc      No UPnP support miniupnp not required
    --disable-upnp-default   (the default) UPnP support turned off by default at runtime
    --enable-upnp-default    UPnP support turned on by default at runtime

To build:

    tar -xzvf miniupnpc-1.6.tar.gz
    cd miniupnpc-1.6
    make
    sudo su
    make install


Berkeley DB
-----------
It is recommended to use Berkeley DB 4.8. If you have to build it yourself:

```bash
SILK_ROOT=$(pwd)

# Pick some path to install BDB to, here we create a directory within the silk directory
BDB_PREFIX="${SILK_ROOT}/db4"
mkdir -p $BDB_PREFIX

# Fetch the source and verify that it is not tampered with
wget 'http://download.oracle.com/berkeley-db/db-4.8.30.NC.tar.gz'
echo '12edc0df75bf9abd7f82f821795bcee50f42cb2e5f76a6a281b85732798364ef  db-4.8.30.NC.tar.gz' | sha256sum -c
# -> db-4.8.30.NC.tar.gz: OK
tar -xzvf db-4.8.30.NC.tar.gz

# Build the library and install to our prefix
cd db-4.8.30.NC/build_unix/
#  Note: Do a static build so that it can be embedded into the exectuable, instead of having to find a .so at runtime
../dist/configure --prefix=/usr/local --enable-cxx
make 
sudo make install

```

**Note**: You only need Berkeley DB if the wallet is enabled (see the section *Disable-Wallet mode* below).

Boost
-----
If you need to build Boost yourself:

    sudo su
    ./bootstrap.sh
    ./bjam install


Security
--------
To help make your Ion installation more secure by making certain attacks impossible to
exploit even if a vulnerability is found, binaries are hardened by default.


Hardening enables the following features:

* Position Independent Executable
    Build position independent code to take advantage of Address Space Layout Randomization
    offered by some kernels. An attacker who is able to cause execution of code at an arbitrary
    memory location is thwarted if he doesn't know where anything useful is located.
    The stack and heap are randomly located by default but this allows the code section to be
    randomly located as well.

    On an Amd64 processor where a library was not compiled with -fPIC, this will cause an error
    such as: "relocation R_X86_64_32 against `......' can not be used when making a shared object;"

    To test that you have built PIE executable, install scanelf, part of paxutils, and use:

        scanelf -e ./xiond

    The output should contain:
     TYPE
    ET_DYN

* Non-executable Stack
    If the stack is executable then trivial stack based buffer overflow exploits are possible if
    vulnerable buffers are found. By default, silk should be built with a non-executable stack
    but if one of the libraries it uses asks for an executable stack or someone makes a mistake
    and uses a compiler extension which requires an executable stack, it will silently build an
    executable without the non-executable stack protection.

    To verify that the stack is non-executable after compiling use:
    `scanelf -e ./xiond`

    the output should contain:
    STK/REL/PTL
    RW- R-- RW-

    The STK RW- means that the stack is readable and writeable but not executable.


Example Build Command
--------------------
Qt Wallet and Deamon, CLI version build:

    qmake && make && cd src && make -f src/makefile.unix

Deamon Only Buld:

    cd src && make -f src/makefile.unix
