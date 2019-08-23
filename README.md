[TOC]



# Compile

## Linux

### Ubuntu (18.04.3)

#### My compilation environment

​	**System**: Python Boost	OpenEXR 	Ilmbase	TBB	GLFW   Log4cplus	Boost_Python	 Zlib	       					Numpy  doxygen

​	**User**: Blosc(1.5.0)	Cppunit(1.13.2)

#### Install Depend

Using UNIX apt-get

```makefile
#!/bin/bash
apt-get install cmake                   # CMake
apt-get install libilmbase-dev          # IlmBase
apt-get install libopenexr-dev          # OpenEXR
apt-get install libtbb-dev              # TBB
apt-get install zlibc                   # zlib
apt-get install libboost-system-dev     # Boost::system
apt-get install libboost-iostreams-dev  # Boost::iostream
apt-get install libboost-python-dev     # Boost::python
apt-get install libboost-thread-dev     # Boost::thread
apt-get install python-dev              # Python
apt-get install python-numpy            # NumPy
apt-get install liblog4cplus-dev        # Log4cplus
apt-get install libglfw3-dev            # GLFW
apt-get install doxygen                 # doxygen
```

Self:

```makefile
mkdir $HOME/Desktop/Dependence/openexr
mkdir $HOME/Desktop/Dependence/ilmbase
mkdir $HOME/Desktop/build_openvdb

# install blosc
cd /tmp
wget https://github.com/Blosc/c-blosc/archive/v1.5.0.zip 
unzip v1.5.0.zip 
cd c-blosc-1.5.0 
cmake -DCMAKE_INSTALL_PREFIX=$HOME/Desktop/Dependence/openexr
make
make install

# install cppunit
cd /tmp
wget http://dev-www.libreoffice.org/src/cppunit-1.13.2.tar.gz 
tar -xvzf cppunit-1.13.2.tar.gz 
cd cppunit-1.13.2 
./configure --prefix=$HOME/Desktop/Dependence/cppunit 
make
make install

# Install Ilmbase
cd /tmp
wget http://download.savannah.nongnu.org/releases/openexr/ilmbase-2.2.0.tar.gz
tar xvf ilmbase-2.2.0.tar.gz
cd ilmbase-2.2.0/
./configure --prefix=$HOME/Desktop/Dependence/ilmbase
make
make install
```



Makefile (origin -> modify)

> ```makefile
> # The directory into which to install libraries, executables and header files
> DESTDIR := /home/ubuntu/Desktop/build_openvdb
> 
> # The directory into which to install libraries (e.g., for Linux multiarch support)
> DESTDIR_LIB_DIR := $(DESTDIR)/lib
> 
> # The parent directory of the boost/ header directory
> BOOST_INCL_DIR := /usr/include/boost
> # The directory containing libboost_iostreams, libboost_system, etc.
> BOOST_LIB_DIR := /usr/lib/x86_64-linux-gnu
> BOOST_LIB := -lboost_iostreams -lboost_system
> BOOST_THREAD_LIB := -lboost_thread
> 
> # The parent directory of the OpenEXR/ header directory
> #EXR_INCL_DIR := /home/ubuntu/Desktop/Dependence/openexr/include
> EXR_INCL_DIR := /usr/include
> # The directory containing IlmImf
> #EXR_LIB_DIR := /home/ubuntu/Desktop/Dependence/openexr/lib
> EXR_LIB_DIR := /usr/lib
> EXR_LIB := -lIlmImf
> 
> # The parent directory of the OpenEXR/ header directory (which contains half.h)
> ILMBASE_INCL_DIR := /home/ubuntu/Desktop/Dependence/ilmbase/include
> # The directory containing libIlmThread, libIlmThread, libHalf etc.
> ILMBASE_LIB_DIR := /home/ubuntu/Desktop/Dependence/ilmbase/lib
> ILMBASE_LIB := -lIlmThread -lIex -lImath
> HALF_LIB := -lHalf
> 
> # The parent directory of the tbb/ header directory
> TBB_INCL_DIR := /usr/include/tbb
> # The directory containing libtbb
> TBB_LIB_DIR := /usr/lib/x86_64-linux-gnu
> TBB_LIB := -ltbb
> 
> # The parent directory of the blosc.h header
> # (leave blank if Blosc is unavailable)
> BLOSC_INCL_DIR := /home/ubuntu/Desktop/Dependence/blosc/include
> # The directory containing libblosc
> BLOSC_LIB_DIR := /home/ubuntu/Desktop/Dependence/blosc/lib
> BLOSC_LIB := -lblosc
> 
> # A scalable, concurrent malloc replacement library
> # such as jemalloc (included in the Houdini HDK) or TBB malloc
> # (leave blank if unavailable)
> CONCURRENT_MALLOC_LIB := -ljemalloc
> #CONCURRENT_MALLOC_LIB := -ltbbmalloc_proxy -ltbbmalloc
> # The directory containing the malloc replacement library
> CONCURRENT_MALLOC_LIB_DIR := $(HDSO)
> 
> # The parent directory of the cppunit/ header directory
> # (leave blank if CppUnit is unavailable)
> CPPUNIT_INCL_DIR := /home/ubuntu/Desktop/Dependence/cppunit/include
> # The directory containing libcppunit
> CPPUNIT_LIB_DIR := /home/ubuntu/Desktop/Dependence/cppunit/lib
> CPPUNIT_LIB := -lcppunit
> 
> # The parent directory of the log4cplus/ header directory
> # (leave blank if log4cplus is unavailable)
> LOG4CPLUS_INCL_DIR := /usr/include
> # The directory containing liblog4cplus
> LOG4CPLUS_LIB_DIR := /usr/lib/x86_64-linux-gnu
> LOG4CPLUS_LIB := -llog4cplus
> 
> # The directory containing glfw.h
> # (leave blank if GLFW is unavailable)
> GLFW_INCL_DIR := /usr/include
> # The directory containing libglfw
> GLFW_LIB_DIR := /usr/lib/x86_64-linux-gnu
> GLFW_LIB := -lglfw
> # The major version number of the GLFW library
> # (header filenames changed between GLFW 2 and 3, so this must be specified explicitly)
> GLFW_MAJOR_VERSION := 3
> 
> # The version of Python for which to build the OpenVDB module
> # (leave blank if Python is unavailable)
> PYTHON_VERSION := 2.7
> # The directory containing Python.h
> PYTHON_INCL_DIR := /usr/include/python2.7
> # The directory containing pyconfig.h
> PYCONFIG_INCL_DIR := $(PYTHON_INCL_DIR)
> # The directory containing libpython
> PYTHON_LIB_DIR := /usr/lib/x86_64-linux-gnu
> PYTHON_LIB := -lpython$(PYTHON_VERSION)
> # The directory containing libboost_python
> BOOST_PYTHON_LIB_DIR := /usr/lib/x86_64-linux-gnu
> BOOST_PYTHON_LIB := -lboost_python-py27
> # The directory containing arrayobject.h
> # (leave blank if NumPy is unavailable)
> NUMPY_INCL_DIR := /usr/lib/python2.7/site-packages/numpy/core/include/numpy/
> # The Epydoc executable
> # (leave blank if Epydoc is unavailable)
> EPYDOC := /usr/bin/pydoc
> # Set PYTHON_WRAP_ALL_GRID_TYPES to "yes" to specify that the Python module
> # should expose (almost) all of the grid types defined in openvdb.h
> # Otherwise, only FloatGrid, BoolGrid and Vec3SGrid will be exposed
> # (see, e.g., exportIntGrid() in python/pyIntGrid.cc).
> # Compiling the Python module with PYTHON_WRAP_ALL_GRID_TYPES set to "yes"
> # can be very memory-intensive.
> PYTHON_WRAP_ALL_GRID_TYPES := no
> 
> # The Doxygen executable
> # (leave blank if Doxygen is unavailable)
> DOXYGEN := doxygen
> 
> ```
>

#### Troubleshooting

* **fatal error: stdlib.h: No such file or directory #include_next <stdlib.h>**

  This error is because the wrong library file was not found or referenced.

  **Solver:** Change all **-isystem** in the Makefile to **-I**

* **.... #include <glu.h>**

```makefile
apt-get install libglu1-mesa-dev freeglut3-dev # glut
```

#### Other help

```makefile
# find
sudo find / -type f -name "stdlib.h" | grep stdlib.h
sudo find / -type f -name "stddef.h" | grep stddef.h

# locate
locate stdlib.h	| grep stdlib.h
```

