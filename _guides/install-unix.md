---
permalink: /guides/install-unix/
toc: true
toc_sticky: true
title: "Install on Linux / macOS"
excerpt: "How to install mHM on Linux or macOS using Conda."
---

  Sebastian Müller\
  Helmholtz Centre for Environmental Research - UFZ

# Overview

Linux, macOS, and WSL users can rely on the Conda ecosystem to install mHM and its runtime dependencies.
We recommend the community-driven [Miniforge](https://github.com/conda-forge/miniforge) installer because it defaults to the `conda-forge` channel, where mHM is published.

## 1. Install Miniforge

Run the following commands in your terminal (works on Linux, macOS, and WSL):

```bash
curl -L -O https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-$(uname)-$(uname -m).sh
bash Miniforge3-$(uname)-$(uname -m).sh
conda config --set auto_activate_base false
```

Accept the default installation path (for example `~/miniforge3`) and allow the installer to initialize Conda for your shell.
Restart the terminal or source the appropriate profile script so that the `conda` command becomes available.

## 2. Create an mHM environment

```bash
conda create -n mhm -c conda-forge mhm ncview
conda activate mhm
```

- The `ncview` package is optional but convenient for quickly browsing NetCDF output (see visualisation tips below).
- Miniforge users already have `conda-forge` configured; specifying `-c conda-forge` keeps the command explicit for other Conda variants.

## 3. Run the standard test

```bash
mhm-download
mhm test_domain/
ncview test_domain/output_b1/mHM_Fluxes_States.nc
```

The first command fetches the official test domain, the second runs mHM, and the third opens the diagnostic NetCDF file in `ncview`.
If you prefer a GUI viewer such as [Panoply](https://www.giss.nasa.gov/tools/panoply/), install it with `conda install -c conda-forge panoply`, skip the `ncview` call, and open `test_domain/output_b1/mHM_Fluxes_States.nc` in your graphical tool of choice.

## 4. Keep environments tidy

- Use `conda deactivate` when you are finished to return to the base shell environment.
- Update packages periodically with `conda update --all -n mhm`.
- Remove the environment with `conda remove -n mhm --all` if you no longer need it.

## 5. Building from source

If you plan to compile the development version of mHM, follow the dependency setup described in the [Compilation Guide](compile) instead of the simple runtime environment above.
