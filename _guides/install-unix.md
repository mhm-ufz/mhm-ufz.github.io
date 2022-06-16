---
permalink: /guides/install-unix/

title: "Install on Linux / MacOS"
excerpt: "How to install mHM on Linux or MacOS using Conda."
---

  Sebastian Müller\
  Helmholtz Centre for Environmental Research - UFZ

# Install mHM with Conda on Linux / MacOS

- The [Conda package manager](https://www.conda.io): open-source, cross-platform, language-agnostic package manager
  - Miniforge (minimal installer for conda using conda-forge): <https://github.com/conda-forge/miniforge>
  - Install Conda from command line: (works on Linux (+WSL) and MacOS)
    ```bash
    curl -L -O https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-latest-$(uname)-$(uname -m).sh
    bash Miniforge3-latest-$(uname)-$(uname -m).sh  # init conda: yes ; restart shell
    conda config --set auto_activate_base false     # otherwise conda is always active
    ```
- Install latest mHM release in a new conda environment:
  ```bash
  conda create -y --prefix ./mhm_env              # environment in local folder
  conda activate ./mhm_env                        # activate this environment
  conda install mhm
  ```
  Now you can run `mhm` by simply calling the provided command anywhere:
  ```bash
  mhm
  ```
- Install latest mHM development version in a new conda environment:
  ```bash
  git clone https://git.ufz.de/mhm/mhm.git
  conda install -y cmake make fortran-compiler netcdf-fortran
  cd mhm                    # enters the directory "mhm"
  source CI-scripts/compile # runs the compilation for mhm
  ```
  Now you can run `mhm` by simply calling the compiled executable:
  ```bash
  ./mhm
  ```
