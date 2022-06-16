---
permalink: /guides/compile/

title: "Compile Instructions"
excerpt: "General instructions on how to compile mHM."
---

The section 'Dependencies' lists the general requirements
for the compilation. The section 'System-dependent dependency installation'
gives some instructions on how to install these dependencies on Windows,
some Linux-distributions and MacOS.
Conda based dependency installation is described in the section 'Conda dependent installation'.


## Dependencies

To clone and compile mHM you need at least the following:

* Fortran compiler: We support [gfortran](https://gcc.gnu.org/fortran/), [nagfor](https://www.nag.com/content/nag-fortran-compiler) and [ifort](https://www.intel.com/content/www/us/en/developer/tools/oneapi/overview.html)
* Build system: We support [make](https://www.gnu.org/software/make/) and [ninja](https://ninja-build.org/)
* [cmake](https://cmake.org/): Software for build automation
* [NetCDF-Fortran](https://github.com/Unidata/netcdf-fortran): NetCDF I/O for Fortran
* [git](https://git-scm.com/): version control system
* (optional) [fypp](https://github.com/aradi/fypp): Fortran pre-processor written in Python


## System-dependent dependency installation

After you installed all dependencies on your system you can proceed with cloning and compiling.


### Unix (Linux / MacOS)

1. MacOS with [homebrew](https://brew.sh) available:
    ```bash
    brew install git gcc netcdf cmake
    ```

1. Ubuntu, Mint and other apt-get based systems with matching repositories:
    ```bash
    sudo apt-get install git gfortran netcdf-bin libnetcdf-dev libnetcdff-dev cmake
    ```

2. Archlinux:
    ```bash
    sudo pacman -S git gcc-libs netcdf-fortran cmake
    ```

3. yum based systems (CentOS, OpenSuse):
    ```bash
    sudo yum -y install git gcc-gfortran netcdf-fortran cmake
    ```

### Windows

On Windows 10 and later we recommend to use [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10) (WSL) to be able to use Linux by e.g. [installing Ubuntu](https://ubuntu.com/tutorials/install-ubuntu-on-wsl2-on-windows-10) there.

Easiest way to do so is:

1. install the [Windows Terminal](https://apps.microsoft.com/store/detail/windows-terminal/9N0DX20HK701)
2. open the Windows Terminal and type:
    ```bash
    wsl --install -d ubuntu
    ```

3. Open Ubuntu from the new entry in the start menu

Then you can follow the install instructions for Ubuntu from above.

If you rather want to use [Cygwin](https://www.cygwin.com/)
(tool providing Linux functionality on Windows), step-by-step guidelines on
how to install all Cygwin libraries can be viewed in [this youtube video](https://youtu.be/FGJOcYEzbP4)
created by Mehmet Cüneyd Demirel (Istanbul Technical University).


### Module systems

If you are on a module system, load the modules gcc or intel depending on your
favorite compiler. Then, load the modules netcdf-fortran and cmake.

These modules will have system specific names, environments, etc.
You may use `module spider` to find the right packages and the
right dependencies, potentially use corresponding wiki pages.


#### On EVE (the cluster at the UFZ)

A set of load-scripts is provided in `hpc-module-loads` (see the [repository](https://git.ufz.de/chs/HPC-Fortran-module-loads) for more details), to load all needed modules for specific compilers:

- Example: GNU 7.3 compiler (`foss/2018b` Toolchain):
  ```bash
  source hpc-module-loads/eve.gcc73
  ```
  or (MPI support)
  ```bash
  source hpc-module-loads/eve.gcc73MPI
  ```


## Conda dependent installation

The simplest way to compile this project on your local computer is to use a [conda](https://docs.conda.io/en/latest/) environment (on Linux (including WSL) or MacOS) provided by [Miniforge](https://github.com/conda-forge/miniforge) to install NetCDF, a Fortran compiler, make, cmake.

On Windows we recommend to use [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10) (WSL) to be able to use Linux (see above) and set up conda there.

You can get the latest version of **Miniforge** with (for Linux/MacOS/WSL):
```bash
wget https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-$(uname)-$(uname -m).sh
bash Miniforge3-$(uname)-$(uname -m).sh
```

To create a (local) conda environment with all dependencies type the following:
```bash
conda create -y --prefix ./fortran_env
conda activate ./fortran_env
conda install -y git cmake make fortran-compiler netcdf-fortran
```

Then you can proceed with cloning and compiling.


## Cloning the repository

First you need to clone the repository (if you already have `git`, otherwise see below):
```bash
git clone https://git.ufz.de/mhm/mhm.git
```

This will give you a new folder `mhm/` containing the whole repository. You can go into it by:
```bash
cd mhm/
```

If you then want to compile a specific version (different from the latest development version), you can check that out with e.g.:
```bash
git checkout v5.11.2
```

Afterwards you can continue with the compilation.


## Installation commands

It could be necessary to set your desired fortran compiler with an environment variable, e.g.:
```bash
export FC=gfortran
```

If you already have all dependencies installed (otherwise see below),
we prepared a set of scripts to automatize the build and compilation process to generate an executable in the root directory with the following naming scheme:

- Release version `mhm`:
  ```bash
  source CI-scripts/compile
  ```
- Debug version `mhm_debug`:
  ```bash
  source CI-scripts/compile_debug
  ```
- Release version with MPI support `mhm_mpi`:
  ```bash
  source CI-scripts/compile_MPI
  ```
- Debug version with MPI support `mhm_mpi_debug`:
  ```bash
  source CI-scripts/compile_MPI_debug
  ```
- Release version with OpenMP support `mhm_openmp`:
  ```bash
  source CI-scripts/compile_OpenMP
  ```
- Debug version with OpenMP support `mhm_openmp_debug`:
  ```bash
  source CI-scripts/compile_OpenMP_debug
  ```

Then you can find an executable `mhm` (or `mhm[_mpi|_openmp][_debug]`) in the current folder.
You can execute it with:
```bash
./mhm
```
