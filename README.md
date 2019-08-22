
C++ & Python API for Scientific I/O with openPMD
================================================

[![Supported openPMD Standard](https://img.shields.io/badge/openPMD-1.0.0--1.1.0-blue.svg)](https://github.com/openPMD/openPMD-standard/releases)
[![Documentation Status](https://readthedocs.org/projects/openpmd-api/badge/?version=latest)](https://openpmd-api.readthedocs.io/en/latest/?badge=latest)
[![Doxygen](https://img.shields.io/badge/API-Doxygen-blue.svg)](https://www.openpmd.org/openPMD-api)
[![Gitter chat](https://img.shields.io/gitter/room/openPMD/API.svg)](https://gitter.im/openPMD/API)
![Supported Platforms][api-platforms]
[![License](https://img.shields.io/badge/license-LGPLv3-blue.svg)](https://www.gnu.org/licenses/lgpl-3.0.html)
[![DOI](https://rodare.hzdr.de/badge/DOI/10.14278/rodare.27.svg)](https://doi.org/10.14278/rodare.27)

[![Linux/OSX Build Status dev](https://img.shields.io/travis/openPMD/openPMD-api/dev.svg?label=dev)](https://travis-ci.org/openPMD/openPMD-api/branches)
[![Windows Build Status dev](https://ci.appveyor.com/api/projects/status/x95q4n620pqk0e0t/branch/dev?svg=true)](https://ci.appveyor.com/project/ax3l/openpmd-api/branch/dev)
[![Coverity Scan Build Status](https://scan.coverity.com/projects/17602/badge.svg)](https://scan.coverity.com/projects/openpmd-openpmd-api)
[![Coverage Status](https://coveralls.io/repos/github/openPMD/openPMD-api/badge.svg)](https://coveralls.io/github/openPMD/openPMD-api)
[![CodeFactor](https://www.codefactor.io/repository/github/openpmd/openpmd-api/badge)](https://www.codefactor.io/repository/github/openpmd/openpmd-api)

[api-platforms]: https://img.shields.io/badge/platforms-linux%20|%20osx%20|%20win-blue.svg "Supported Platforms"

This library provides a high-level API for writing and reading scientific data according to [openPMD](https://www.openpmd.org/).
It allows to build logical file structures which drive scientific I/O libraries such as HDF5 and ADIOS through a common, intuitive interface.
Where supported, openPMD-api implements both serial and MPI parallel I/O capabilities.

## Usage

### C++

[![C++11][api-cpp]](https://isocpp.org/) ![C++11 API: Alpha][dev-alpha]

[api-cpp]: https://img.shields.io/badge/language-C%2B%2B11-yellowgreen.svg "C++11 API"
[dev-alpha]: https://img.shields.io/badge/phase-alpha-yellowgreen.svg "Status: Alpha"

```cpp
#include <openPMD/openPMD.hpp>
#include <iostream>

// ...

auto s = openPMD::Series("samples/git-sample/data%T.h5", openPMD::AccessType::READ_ONLY);

for( auto const& i : s.iterations ) {
    std::cout << "Iteration: " << i.first << "\n";

    for( auto const& m : i.second.meshes ) {
        std::cout << "  Mesh '" << m.first << "' attributes:\n";
        for( auto const& val : m.second.attributes() )
            std::cout << "    " << val << '\n';
    }

    for( auto const& p : i.second.particles ) {
        std::cout << "  Particle species '" << p.first << "' attributes:\n";
        for( auto const& val : p.second.attributes() )
            std::cout << "    " << val << '\n';
    }
}
```

### Python

[![Python3][api-py3]](https://www.python.org/) ![Python3 API: Alpha][dev-alpha]

[api-py3]: https://img.shields.io/badge/language-Python3-yellowgreen.svg "Python3 API"


```py
import openpmd_api

# ...

series = openpmd_api.Series("samples/git-sample/data%T.h5", openPMD.Access_Type.read_only)

for k_i, i in series.iterations.items():
    print("Iteration: {0}".format(k_i))

    for k_m, m in i.meshes.items():
        print("  Mesh '{0}' attributes:".format(k_m))
        for a in m.attributes:
            print("    {0}".format(a))

    for k_p, p in i.particles.items():
        print("  Particle species '{0}' attributes:".format(k_p))
        for a in p.attributes:
            print("    {0}".format(a))
```

### More!

Curious?
Our manual shows full [read & write examples](https://openpmd-api.readthedocs.io/en/latest/usage/firstwrite.html), both serial and MPI-parallel!

## Dependencies

Required:
* CMake 3.11.0+
* C++11 capable compiler, e.g. g++ 4.8+, clang 3.9+, VS 2015+

Shipped internally in `share/openPMD/thirdParty/`:
* [MPark.Variant](https://github.com/mpark/variant) 1.4.0+ ([BSL-1.0](https://github.com/mpark/variant/blob/master/LICENSE.md))
* [Catch2](https://github.com/catchorg/Catch2) 2.6.1+ ([BSL-1.0](https://github.com/catchorg/Catch2/blob/master/LICENSE.txt))
* [pybind11](https://github.com/pybind/pybind11) 2.3.0+ ([new BSD](https://github.com/pybind/pybind11/blob/master/LICENSE))
* [NLohmann-JSON](https://github.com/nlohmann/json) 3.7.0+ ([MIT](https://github.com/nlohmann/json/blob/develop/LICENSE.MIT))

Optional I/O backends:
* [JSON](https://en.wikipedia.org/wiki/JSON)
* [HDF5](https://support.hdfgroup.org/HDF5) 1.8.13+
* [ADIOS1](https://www.olcf.ornl.gov/center-projects/adios) 1.13.1+
* [ADIOS2](https://github.com/ornladios/ADIOS2) 2.4.0+ (*not yet implemented*)

while those can be built either with or without:
* MPI 2.1+, e.g. OpenMPI 1.6.5+ or MPICH2

Optional language bindings:
* Python:
  * Python 3.5 - 3.7
  * pybind 2.3.0+
  * numpy 1.15+
  * mpi4py 2.1+

## Installation

[![Spack Package](https://img.shields.io/badge/spack.io-openpmd--api-brightgreen.svg)](https://spack.readthedocs.io/en/latest/package_list.html#openpmd-api)
[![Conda Package](https://img.shields.io/badge/conda.io-openpmd--api-brightgreen.svg)](https://anaconda.org/conda-forge/openpmd-api)
[![PyPI Package](https://img.shields.io/badge/pypi.org-openpmd--api-yellow.svg)](https://pypi.org/project/openPMD-api)
[![From Source](https://img.shields.io/badge/from_source-CMake-brightgreen.svg)](https://cmake.org)

Choose *one* of the install methods below to get started:

### [Spack](https://spack.io)

[![Spack Version](https://img.shields.io/spack/v/openpmd-api.svg)](https://spack.readthedocs.io/en/latest/package_list.html#openpmd-api)
[![Spack Status](https://img.shields.io/badge/method-recommended-brightgreen.svg)](https://spack.readthedocs.io/en/latest/package_list.html#openpmd-api)

```bash
# optional:               +python +adios1 -mpi
spack install openpmd-api
spack load -r openpmd-api
```

### [Conda](https://conda.io)

[![Conda Version](https://img.shields.io/conda/vn/conda-forge/openpmd-api.svg)](https://anaconda.org/conda-forge/openpmd-api)
[![Conda Status](https://img.shields.io/badge/method-recommended-brightgreen.svg)](https://anaconda.org/conda-forge/openpmd-api)
[![Conda Downloads](https://img.shields.io/conda/dn/conda-forge/openpmd-api.svg)](https://anaconda.org/conda-forge/openpmd-api)

```bash
# optional:            OpenMPI support  =*=mpi_openmpi*
# optional:              MPICH support  =*=mpi_mpich*
conda install -c conda-forge openpmd-api
```

### [PyPI](https://pypi.org)

[![PyPI Version](https://img.shields.io/pypi/v/openPMD-api.svg)](https://pypi.org/project/openPMD-api)
[![PyPI Format](https://img.shields.io/pypi/format/openPMD-api.svg)](https://pypi.org/project/openPMD-api)
[![PyPI Status](https://img.shields.io/badge/method-experimental-yellow.svg)](https://pypi.org/project/openPMD-api)
[![PyPI Downloads](https://img.shields.io/pypi/dm/openPMD-api.svg)](https://pypi.org/project/openPMD-api)

Behind the scenes, this install method *compiles from source* against the found installations of HDF5, ADIOS and/or MPI (in system paths, from other package managers, or loaded via a module system, ...).
The current status for this install method is *experimental*.
Please feel free to [report](https://github.com/openPMD/openPMD-api/issues/new/choose) how this works for you.

```bash
# optional:             --user
pip install openpmd-api
```

or with MPI support:
```bash
# optional:                                                  --user
openPMD_USE_MPI=ON pip install openpmd-api --no-binary :all:
```

### From Source

[![Source Status](https://img.shields.io/badge/method-advanced-brightgreen.svg)](https://cmake.org)

openPMD can then be installed using [CMake](https://cmake.org/):

```bash
git clone https://github.com/openPMD/openPMD-api.git

mkdir openPMD-api-build
cd openPMD-api-build

# optional: for full tests, with unzip
../openPMD-api/.travis/download_samples.sh

# for own install prefix append:
#   -DCMAKE_INSTALL_PREFIX=$HOME/somepath
# for options append:
#   -DopenPMD_USE_...=...
# e.g. for python support add:
#   -DopenPMD_USE_PYTHON=ON -DPYTHON_EXECUTABLE=$(which python)
cmake ../openPMD-api

cmake --build .

# optional
ctest

# sudo might be required for system paths
cmake --build . --target install
```

The following options can be added to the `cmake` call to control features.
CMake controls options with prefixed `-D`, e.g. `-DopenPMD_USE_MPI=OFF`:

| CMake Option                 | Values           | Description                                                                  |
|------------------------------|------------------|------------------------------------------------------------------------------|
| `openPMD_USE_MPI`            | **AUTO**/ON/OFF  | Parallel, Multi-Node I/O for clusters                                        |
| `openPMD_USE_JSON`           | **AUTO**/ON/OFF  | JSON backend (`.json` files)                                                 |
| `openPMD_USE_HDF5`           | **AUTO**/ON/OFF  | HDF5 backend (`.h5` files)                                                   |
| `openPMD_USE_ADIOS1`         | **AUTO**/ON/OFF  | ADIOS1 backend (`.bp` files)                                                 |
| `openPMD_USE_ADIOS2`         | AUTO/ON/**OFF**  | ADIOS2 backend (`.bp` files) <sup>1</sup>                                    |
| `openPMD_USE_PYTHON`         | **AUTO**/ON/OFF  | Enable Python bindings                                                       |
| `openPMD_USE_INVASIVE_TESTS` | ON/**OFF**       | Enable unit tests that modify source code <sup>2</sup>                       |
| `openPMD_USE_VERIFY`         | **ON**/OFF       | Enable internal VERIFY (assert) macro independent of build type <sup>3</sup> |
| `PYTHON_EXECUTABLE`          | (first found)    | Path to Python executable                                                    |

<sup>1</sup> *not yet implemented*
<sup>2</sup> *e.g. changes C++ visibility keywords, breaks MSVC*
<sup>3</sup> *this includes most pre-/post-condition checks, disabling without specific cause is highly discouraged*

Additionally, the following libraries are shipped internally.
The following options allow to switch to external installs:

| CMake Option                    | Values     | Library       | Version |
|---------------------------------|------------|---------------|---------|
| `openPMD_USE_INTERNAL_VARIANT`  | **ON**/OFF | MPark.Variant |  1.4.0+ |
| `openPMD_USE_INTERNAL_CATCH`    | **ON**/OFF | Catch2        |  2.6.1+ |
| `openPMD_USE_INTERNAL_PYBIND11` | **ON**/OFF | pybind11      |  2.3.0+ |
| `openPMD_USE_INTERNAL_JSON`     | **ON**/OFF | NLohmann-JSON |  3.7.0+ |

By default, this will build as a shared library (`libopenPMD.[so|dylib|dll]`) and installs also its headers.
In order to build a static library, append `-DBUILD_SHARED_LIBS=OFF` to the `cmake` command.
You can only build a static or a shared library at a time.

By default, the `Release` version is built.
In order to build with debug symbols, pass `-DCMAKE_BUILD_TYPE=Debug` to your `cmake` command.

By default, tests and examples are built.
In order to skip building those, pass `-DBUILD_TESTING=OFF` or `-DBUILD_EXAMPLES` to your `cmake` command.

## Linking to your project

The install will contain header files and libraries in the path set with `-DCMAKE_INSTALL_PREFIX`.

### CMake

If your project is using CMake for its build, one can conveniently use our provided `openPMDConfig.cmake` package which is installed alongside the library.

First set the following environment hint if openPMD-api was *not* installed in a system path:

```bash
# optional: only needed if installed outside of system paths
export CMAKE_PREFIX_PATH=$HOME/somepath:$CMAKE_PREFIX_PATH
```

Use the following lines in your project's `CMakeLists.txt`:
```cmake
# supports:                       COMPONENTS MPI NOMPI JSON HDF5 ADIOS1 ADIOS2
find_package(openPMD 0.1.0 CONFIG)

if(openPMD_FOUND)
    target_link_libraries(YourTarget PRIVATE openPMD::openPMD)
endif()
```

*Alternatively*, add the openPMD-api repository source directly to your project and use it via:
```cmake
add_subdirectory("path/to/source/of/openPMD-api")

target_link_libraries(YourTarget PRIVATE openPMD::openPMD)
```

### Manually

If your (Linux/OSX) project is build by calling the compiler directly or uses a manually written `Makefile`, consider using our `openPMD.pc` helper file for `pkg-config` which are installed alongside the library.

First set the following environment hint if openPMD-api was *not* installed in a system path:

```bash
# optional: only needed if installed outside of system paths
export PKG_CONFIG_PATH=$HOME/somepath/lib/pkgconfig:$PKG_CONFIG_PATH
```

Additional linker and compiler flags for your project are available via:
```bash
pkg-config --libs openPMD
# -L${HOME}/somepath/lib -lopenPMD

# if you build openPMD-api as static library with BUILD_SHARED_LIBS=OFF
pkg-config --libs --static openPMD
# -L/usr/local/lib -L/usr/lib/x86_64-linux-gnu/openmpi/lib -lopenPMD -pthread /usr/lib/libmpi.so -pthread /usr/lib/x86_64-linux-gnu/openmpi/lib/libmpi_cxx.so /usr/lib/libmpi.so /usr/lib/x86_64-linux-gnu/hdf5/openmpi/libhdf5.so /usr/lib/x86_64-linux-gnu/libsz.so /usr/lib/x86_64-linux-gnu/libz.so /usr/lib/x86_64-linux-gnu/libdl.so /usr/lib/x86_64-linux-gnu/libm.so -pthread /usr/lib/libmpi.so -pthread /usr/lib/x86_64-linux-gnu/openmpi/lib/libmpi_cxx.so /usr/lib/libmpi.so

pkg-config --cflags openPMD
# -I${HOME}/somepath/include
```
