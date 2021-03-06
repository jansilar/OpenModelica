# OpenModelica
[OpenModelica](https://openmodelica.org) is an open-source Modelica-based modeling and simulation environment intended for industrial and academic usage.

## Dependencies

Many software packages are included inside the repositories.
To get everything running, you will need a few extras:
- autoconf, automake, libtool, g++, gfortran (pretty standard compilers)
- boost (optional; used with configure --with-cppruntime)
- [clang](http://clang.llvm.org/), clang++ (optional, but *highly recommended*)
- [cmake](http://www.cmake.org)
- hwloc (optional; queries the number of hardware CPU cores instead of logical CPU cores)
- Java JRE (JDK is option; compiles the Java CORBA interface)
- Lapack/BLAS
- [lpsolve55](http://lpsolve.sourceforge.net)
- libhdf5 (optional part of the [MSL](https://github.com/modelica/Modelica) tables library supported by few other Modelica tools, so it does not do much)
- libexpat (it's actually included in the FMIL sources which are included... but we do not compile those and it's better to use the OS-provided dynamically linked version)
- ncurses, readline (optional, used by OMShell-terminal)
- omniORB (optional; CORBA is used by OMOptim and OMShell)
- OpenSceneGraph (optional, part of Modelica3D which should be compiled but is actually not yet compiled)
- Qt4, Webkit
- [Sundials](http://www.llnl.gov/CASC/sundials/) (optional; adds more numerical solvers to the simulation runtime)

## Compilation (Linux/OSX)

```bash
$ autoconf
$ ./configure CC=clang CXX=clang++
$ make -j8
$ build/bin/omc --version
$ (cd testsuite/partest && ./runtests.pl -j8)
```

## Working with the repository

OpenModelica.git is a superproject. Clone the project using one of:
```bash
# Faster pulling by using openmodelica.org read-only mirror (low latency in Europe; very important when updating all submodules)
# Replace the openmodelica.org pull URL with https://github.com/OpenModelica/OpenModelica.git if you want to pull directly from github
# The default choice is to push to your fork on github.com (SSH). Replace MY_FORK with OpenModelica to push directly to the OpenModelica repositories (if you have access)
MY_FORK=MyName ; git clone https://openmodelica.org/git-readonly/OpenModelica.git --recursive && (cd OpenModelica && git remote set-url --push origin git@github.com:$MY_FORK/OpenModelica.git && git submodule foreach --recursive 'git remote set-url --push origin `git config --get remote.origin.url | sed s,^.*/,git@github.com:'$MY_FORK'/,`')
```
To keep the project updated, use something like:
```bash
# Note: Changes the head to the latest one; your changes might be lost
git pull --recurse-submodules && git submodule update --recursive
```
If you have push access to the submodules, you can push them all together and let [hudson](https://test.openmodelica.org/hudson/) run the tests before this project is updated (only necessary if you change an interface or the test suite at the same time as [OMCompiler](https://github.com/OpenModelica/OMCompiler)):
```bash
git submodule foreach --recursive "git push"
```
If you are a developer and want to track the latest heads, use:
```bash
# After cloning
git submodule foreach --recursive "git checkout master"
# To update; you will need to merge each submodule, but your changes will remain
git submodule foreach --recursive "git pull"
```
