# Building ispc

OS : Ubuntu 18.04

# Prerequisties
Prerequisites
    CMake 3.8 or later - http://www.cmake.org/cmake/resources/software.html/
    Python 2.7 - http://www.python.org/download/
    Bison 2.4 or later. (Unfortunately, the version that ships with macOS is 2.3). Bison can be downloaded from http://www.gnu.org/software/bison/ or installed via third-party package manager (for example, Cygwin package manager on Windows, HomeBrew on macOS).
    Flex 2.5 or later. (Run flex --version to check.) Flex can be downloaded from http://flex.sourceforge.net/ (or use Cygwin on Windows, HomeBrew on macOS).
    SVN and GIT clients to check out LLVM and ISPC sources accordingly.

```bash
sudo apt-get install cmake
sudo apt-get install python
sudo apt-get install bison
sudo apt-get install git
```

# 1. Clone from git hub page

```bash
mkdir ~/project
cd ~/project
git clone https://github.com/ispc/ispc
cd ispc
```

# 2. Alloc.py
Intel recommend using alloc.py

Brefore, use it, You should  SetEnviornment Variable

```bash
export ISPC_HOME=~/project/ispc
export LLVM_HOME=~project/ispc/llvm_build
```

Then, Run script.

```bash
alloy.py -b --version=8.0 --git
# Start to build LLVM
```

## 3. After Build LLVM
```bash
export PATH="$PATH:~/project/ispc/llvm_build/build-8.0/"
```

# Build

```bash
cd ~/project/ispc
mkdir build
cmake -DCMAKE_INSTALL_PREFIX=/absolute/path/to/ispc/bin -DCMAKE_CXX_COMPILER=clang++ path/to/ispc/source/root 
make
```

# Add PATH

You should have an ispc binary in the top-level ispc directory when the build completes. Add /absolute/path/to/ispc/bin to your PATH and you're ready to go!

# reference
https://github.com/ispc/ispc/wiki/Building-ispc