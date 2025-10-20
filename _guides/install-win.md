---
permalink: /guides/install-win/
toc: true
toc_sticky: true
title: "Install on Windows 11"
excerpt: "How to install mHM on Windows 11 using Conda or Windows Subsystem for Linux."
---

  Pallav Kumar Shrestha, Sebastian Müller\
  Helmholtz Centre for Environmental Research - UFZ

# Overview

Windows 11 users can install mHM with the Conda package manager provided by [Miniforge](https://github.com/conda-forge/miniforge) either directly on Windows or inside Windows Subsystem for Linux (WSL).
The sections below walk through both options and conclude with the recommended test commands.

## Native Windows 11 with Conda

### 1. Install miniforge

1. Download the latest `Miniforge3-Windows-x86_64.exe` installer from the [Miniforge releases page](https://github.com/conda-forge/miniforge/releases/latest).
2. Run the installer, choose **Just Me**, and keep the default installation path (for example `C:\Users\<user>\Miniforge3`).
3. Allow the installer to initialize Conda for the Miniforge Prompt (or run `conda init` when asked).
4. Open the **Miniforge Prompt** from the Start menu once the installation finishes.

### 2. Create an mHM environment

```bash
conda create -n mhm -c conda-forge mhm
conda activate mhm
```

You now have a Windows-native `mhm` command available in the activated prompt.

### 3. Run the standard test

Download the test domain, execute mHM, and inspect the output directory:

```bash
mhm-download
mhm test_domain/
```

The generated NetCDF file is stored in `test_domain/output_b1/mHM_Fluxes_States.nc`.

### 4. Visualise NetCDF output with Panoply

`ncview` is not available on native Windows, so we recommend [Panoply](https://www.giss.nasa.gov/tools/panoply/):

1. Install Panoply via Conda (`conda install -c conda-forge panoply`) or download the Windows installer from NASA.
2. Start Panoply, select **File → Open**, and browse to `test_domain/output_b1/mHM_Fluxes_States.nc`.
3. Inspect the example output and close Panoply when finished.

Deactivate the Conda environment once you are done:

```bash
conda deactivate
```

## Windows Subsystem for Linux (WSL) with GUI support

WSL on Windows 11 ships with WSLg, which adds Wayland and X11 support so graphical tools such as `ncview` run without extra X servers.
Follow the next steps if you prefer a Linux environment or need Linux-only tooling.

### 1. Enable WSL and install Ubuntu

Microsoft documents several ways to enable WSL, including installing directly from the Microsoft Store—see the [official WSL installation guide](https://learn.microsoft.com/windows/wsl/install) for the latest options.
One quick path is:

1. Open **Windows Terminal** (or PowerShell) as Administrator.
2. Run `wsl --install -d Ubuntu` to install the latest Ubuntu distribution.
3. Restart Windows when prompted so that the kernel and drivers are enabled.
4. After reboot, launch **Ubuntu** from the Start menu and create your Linux user when asked.
5. (Recommended) Update WSL components with `wsl --update`.

WSLg activates automatically; no additional configuration is required for X11 applications.

### 2. Install Miniforge inside WSL

In the Ubuntu terminal:

```bash
sudo apt update
sudo apt install -y curl ca-certificates
curl -L -O https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Linux-$(uname -m).sh
bash Miniforge3-Linux-$(uname -m).sh
source ~/miniforge3/bin/activate
conda config --set auto_activate_base false
conda deactivate
```

Restart the terminal or run `source ~/miniforge3/bin/activate` whenever you need Conda.

### 3. Create an mHM environment with ncview

```bash
conda create -n mhm -c conda-forge mhm ncview
conda activate mhm
```

### 4. Run the standard test

```bash
mhm-download
mhm test_domain/
ncview test_domain/output_b1/mHM_Fluxes_States.nc
```

`ncview` opens in a Windows window through WSLg. Close it when you are done reviewing the output.

### 5. Working with files across Windows and WSL

- Your Windows drives are available under `/mnt/c`, `/mnt/d`, … inside Ubuntu.
- You can keep the model domain on the Windows side and run `mhm` from WSL; the output stays accessible from both systems.
- Deactivate the environment when finished: `conda deactivate`.

## Next steps

- Follow the [Linux / macOS guide](install-unix) for additional Conda tips that also apply inside WSL.
- Consult the [Compilation Guide](compile) if you need to build mHM from source on Windows or WSL.
