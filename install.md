<!-- --- title: Installation -->

You need c++ compiler, git, cmake. Works under linux and mac osx. For Windows ... (it's a joke?).

Main dependencies are: ODB, ITK, DCMTK, Ceres-solver, Boost.

### Install ODB <img src="http://www.codesynthesis.com/media/logo-large-w.png" width="48">
* For object-oriented database management : "_ODB is an object-relational mapping (ORM) system for C++. It provides tools, APIs, and library support that allow you to persist C++ objects to a relational database (RDBMS) without having to deal with tables, columns, or SQL and without manually writing any of the mapping code._"
* See http://www.codesynthesis.com/products/odb
* Work with version >= 2.4.0
* Need 3 parts to be installed: 1) odb compiler 2) libodb and 3) libodb-sqlite
* for Mac osx:
  * Download and uncompress odb compiler binaries: http://www.codesynthesis.com/download/odb/2.4/odb-2.4.0-i686-macosx.tar.bz2
  * Download, compile from source for libodb and libodb)sqlite: http://www.codesynthesis.com/download/odb/2.4/libodb-sqlite-2.4.0.tar.bz2 and http://www.codesynthesis.com/download/odb/2.4/libodb-2.4.0.tar.bz2

<!-- , use homebrew https://github.com/Max13/homebrew-odb -->
<!--  * `brew tap max13/odb` -->
<!--  * `brew install odb` -->
<!--  * `brew install libodb` -->
<!--  * `brew link --overwrite  libodb` -->
<!--  * `brew install libodb-sqlite` -->
<!--  * `brew link --overwrite  libodb-sqlite` -->

### Install ITK <img src="images/itk_logo.png" width="85">
* For image processing
* See www.itk.org
* Work with version >= 4.5.2
* (Make sure you use `BUILD_SHARED_LIBS=ON`)
* because c++11 is needed for syd, you need to compile ITK also with c++11. Use:`ccmake -DCMAKE_CXX_FLAGS=-std=c++11 ../InsightToolkit-4.8.0`. Note that this option *must* be set before any `ccmake`.
* You may want to set the env variable: `export ITK_DIR=/my_path/build-itk`

### Install DCMTK <img src="images/dcmtk_logo.gif" width="48">
* For Dicom processing
* See http://dicom.offis.de/dcmtk.php.en
* Work with version >= 3.6.0
* DCMTK 3.6.0, latest public version may not compile with latest version of gcc installed in your system. Please consider using newer shapshot of DCMTK.
* You need to activate BUILD_SHARED_LIBS=ON , consider using CMake instead of the recommended './configure' file

### Install Boost (devel)
* Extended C++ library
* Version ?

### Install Ceres-solver
* For fit and optimisation
* Version ?

### Download and compile the source
* `git clone https://github.com/OpenSyd/syd.git`
* `mkdir build ; cd build`
* `ccmake ../syd`
Provide ITK_DIR, and other path to dependence libraries, then type 'c' and 'g' to configure and generate makefiles)
* `make -j 4`
* Optional: `make test`

Once compiled, the libraries are in `build/lib` and the executable in `build/bin`. You **need** to set the environment variable `SYD_PLUGIN` to point to the libraries, for example:

``` sh
export SYD_PLUGIN=/my_path/build/lib:${SYD_PLUGIN}
```
