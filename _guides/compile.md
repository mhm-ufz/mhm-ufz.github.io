---
permalink: /guides/compile/
toc: true
toc_sticky: true
title: "Compilation Guide"
excerpt: "General instructions on how to compile mHM from source."
---

# Overview

Compiling mHM from source lets you track the latest development version, apply custom patches, and fine-tune build options such as MPI or OpenMP support.
The instructions below assume you use Conda (via [Miniforge](https://github.com/conda-forge/miniforge)) to manage compilers and libraries on every platform.
If you only need the pre-built release, follow the platform-specific installation guides instead: [Windows 11](install-win) or [Linux / macOS](install-unix).

## Recommended tooling

- [Visual Studio Code](https://code.visualstudio.com/) with the [Modern Fortran](https://marketplace.visualstudio.com/items?itemName=fortran-lang.linter-gfortran) extension for syntax highlighting, linting, and CMake integration.
- Conda environments created with Miniforge for isolated dependency stacks.
- Git for version control and collaboration.

## Dependency summary

To build mHM you need the following tools and libraries:

- Fortran compiler (`gfortran`, `ifort`, or `nagfor`); the examples below use GNU compilers.
- C compiler (`gcc`, `clang`, or MSVC) for C interoperability libraries.
- Build system (`ninja` or `make`).
- [CMake](https://cmake.org/) 3.18 or newer.
- [pkg-config](https://www.freedesktop.org/wiki/Software/pkg-config/) to locate NetCDF and other libraries.
- [NetCDF-Fortran](https://github.com/Unidata/netcdf-fortran) (the library, not just the command-line tools).
- [fypp](https://github.com/aradi/fypp) if you want to regenerate pre-processed sources.
- Git, curl/wget, and other developer utilities as needed.

## Prepare dependencies with Conda

The commands below create dedicated build environments. Feel free to reuse the runtime environments from the installation guides if you already have the required packages.

### Linux / macOS / WSL

```bash
conda create -n mhm-dev -c conda-forge compilers pkg-config cmake make ninja netcdf-fortran fypp git
conda activate mhm-dev
```

- The `compilers` metapackage pulls in recent GNU/Clang toolchains including `gfortran`, `gcc`, and `g++`.
- `make` and `ninja` are both installed so you can choose your preferred generator.
- On macOS you may need to allow the Conda-provided toolchain by running `xcode-select --install` once if requested.

### Windows 11 (Miniforge)

```bash
conda create -n mhm-dev -c conda-forge gfortran_win-64 gcc_win-64 pkg-config cmake ninja netcdf-fortran fypp git
conda activate mhm-dev
```

- `gfortran_win-64` provides the Fortran compiler; `gcc_win-64` supplies a matching GNU C/C++ toolchain compatible with the NetCDF libraries.
- `ninja` is required because CMake performs best on Windows with the Ninja generator.
- If you prefer Microsoft Visual C++ instead of the GNU toolchain, install the *Build Tools for Visual Studio* separately and ensure `cl.exe` is available in the developer command prompt before calling CMake.

### Keep environments organised

- Update packages with `conda update --all -n mhm-dev`.
- Deactivate the environment when finished: `conda deactivate`.
- Remove it with `conda remove -n mhm-dev --all` if you want to start over.
- If Git is already available on your system, you can omit it from the `conda create` command.

## Alternative dependency sources (optional)

You can also install the required dependencies via system package managers:

- **macOS (Homebrew):**
  ```bash
  brew install git gcc netcdf cmake ninja pkg-config fypp
  ```
- **Ubuntu, Debian, or other APT-based distributions:** use the official repositories or build newer compiler and NetCDF versions from source if necessary.
- **Cluster module systems:** load appropriate compiler, NetCDF, pkg-config, and CMake modules before building.

The Conda approach remains the most portable, especially on shared workstations and Windows.

## Clone the repository

```bash
git clone https://git.ufz.de/mhm/mhm.git
cd mhm
```

To build a specific release, check it out before configuring:

```bash
git checkout v5.12.0
```

Replace the version tag with the release you require.

## Configure and build

You can either use the helper scripts maintained in the repository or run CMake manually.

### Using helper scripts

```bash
source CI-scripts/compile              # Release build
source CI-scripts/compile_debug        # Debug build
source CI-scripts/compile_MPI          # Release + MPI
source CI-scripts/compile_OpenMP       # Release + OpenMP
```

Each script creates a build directory (for example `release/`) and produces the corresponding executable (`mhm`, `mhm_debug`, `mhm_mpi`, …).
Ensure your Conda environment is active before invoking the script so the compilers and libraries are discovered.

### Running CMake manually

```bash
export FC=$(which gfortran)  # Optional: override the Fortran compiler explicitly

cmake -S . -B build -DCMAKE_BUILD_TYPE=Release
cmake --build build
```

- On Windows add `-G Ninja` to the first command to select the Ninja generator:
  ```bash
  cmake -S . -B build -G Ninja -DCMAKE_BUILD_TYPE=Release
  cmake --build build
  ```
- Enable optional features by adding `-DMHM_ENABLE_MPI=ON`, `-DMHM_ENABLE_OPENMP=ON`, or other CMake cache variables as needed.
- Regenerate the build system after changing compiler-related environment variables.

## Install the executable (optional)

To make the executable available as a command inside your Conda environment:

```bash
cmake --install build --prefix "$CONDA_PREFIX"
```

Replace `build` with the actual build directory if you used the helper scripts (for example `release`).

## Verify the build

Run the standard test sequence from the root of the repository or any working directory after activating the environment where you installed mHM:

```bash
mhm-download
mhm test_domain/
ncview test_domain/output_b1/mHM_Fluxes_States.nc
```

If `ncview` is not available on your platform, open the NetCDF file with a GUI tool such as Panoply instead.

## Next steps

- Keep the Conda environment for subsequent development or create project-specific environments if you maintain multiple branches.
- Consult the repository documentation and `CMakeLists.txt` for advanced build options or integration with HPC environments.
