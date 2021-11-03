---
permalink: /guides/install-win/

title: "Install on Windows 10 with WSL2"
excerpt: "How to install mHM on Windows 10 using Windows Subsystem for Linux and Conda."
---

  Pallav Kumar Shrestha, Sebastian Müller\
  Helmholtz Centre for Environmental Research - UFZ

# Overview

## About this Document

This document is a guide to install mHM in machines with Windows 10 as operating system (OS). The installation consists of mainly two parts:

1.  Installation of Windows Subsystem for Linux (WSL2)

2.  Setting up Conda environment followed by installation of mHM

## WSL2 in short

WSL2 allows users to add a Linux OS of choice within Windows 10 OS. This is a similar concept to Virtual Machines, VMs (e.g. Oracle Virtual Box) or Cygwin - to generate a Linux environment to work with Linux based software like mHM, within a Windows system.

The following points make WSL2 a better choice over VMs and Cygwin for
Windows 10 OS:

1. WSL2 is offered officially by Microsoft. In fact, the WSL2 setup in this document is based on the guide in [Microsoft's web page](https://docs.microsoft.com/en-us/windows/wsl/install-win10)!
2. With WSL2, one can use the Bash shell as if you were on a computer with Linux installed as its primary OS. This implies full computer resources at disposal while running mHM.

## mHM in Windows 10

Although popular Linux systems (e.g. Ubuntu) have GUI, WSL2 provides only the CL (command line) version of the Linux OS. CL is similar to command prompt of Windows OS. CL Linux is sufficient for mHM to run.

Interestingly, WSL2 allows users to access the full computer storage from the Linux OS. Thus, one possible working procedure would be to plan the working directory within Windows partitions (C:, D:) which are accessible from Linux. In this way, the mHM runs are made from Linux while the data can be visualized or further processed from both Windows and Linux.

Some of common tools to visualize mHM netCDF (input/ output) files are:

1. [ncView](http://meteora.ucsd.edu/~pierce/ncview_home_page.html) for Linux

2. [Panoply](https://www.giss.nasa.gov/tools/panoply/) for Windows, MacOS, Linux

# Installation

## WSL2 setup in Windows 10

Steps to install WSL2 in Windows 10:

1.  Type `PowerShell` in windows search bar. Open PowerShell as Administrator i.e. right click and *run as Administrator*.

2.  Type (or copy paste) the following in the PowerShell and hit enter:

    ```bash
    dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
    ```

    The Windows Subsystem for Linux (WSL) has now been enabled. First step complete!

3.  In the same PowerShell, type (or copy paste) the following and hit enter:

    ```bash
    dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
    ```

    Your Windows how has virtualization capabilities. Step two complete!

4.  For steps 1 and 2 to take into effect, **restart your computer** before proceeding further.

5.  After the restart, download the following package: https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi

6.  Double click on the package (**wsl_update_x64.msi**) once downloaded. Select *yes* when asked for approval to run the package.

    Great! You just upgraded your WSL to WSL2, which is the latest version.

7.  Open the PowerShell as before (as Administrator) and type or copy paste the following and hit enter:

    ```bash
    wsl --set-default-version 2
    ```

    You have just set WSL2 as the default one. Nice.

8.  Type **Microsoft Store** in the windows search bar. Open Microsoft Store and search for your favorite Linux distribution. If you are new to Linux distributions, just type **Ubuntu**. Click on one of the Ubuntu options and click on **get**. At the time of preparing this guide, the latest Ubuntu version was **20.04LTS**.

    Awesome! You just downloaded and installed a Linux OS as a subsystem in Windows 10 OS.

9.  Type **Ubuntu** (or whichever Linux distro you opted) in the windows search bar and click to open. In the first instance, the Linux will setup the OS and takes few minutes to complete. Be patient.

10. If everything goes well, you should see **Installation successful!** message followed by new login setup. Choose your username and password for your Linux OS. Needless to say, you need to remember this login every time you enter your Linux.

    Welcome on board to Linux world! What you are seeing is the command line (CL) system of Linux, similar to the old MS-DOS or command prompt or the new PowerShell of Windows 10. The Linux CL, a.k.a. **terminal**, is enough for you to run and even process mHM input output.

    **Note**: If you find the visuals of the terminal boring, rest assured that you are not alone who feels this way. You can changethe looks of the terminal, font, etc. by right clicking the header bar of the window and accessing **properties**. Tweak your terminal and keep yourself motivated!

## Setting mHM environment with Conda in the new Linux

Phew! You finally have a Linux subsystem running in Windows 10. But before installing mHM, we need to setup an environment for it to be installed and run. mHM installation (or compilation) and mHM run require many libraries (or dependencies) to work. Activation of all of these dependencies sets the environment for mHM. Manually compiling each of these dependencies is not easy. Here is where Conda comes into play. Conda creates the mhm specific environment for you, hassle-free. Follow the procedure below carefully:

1.  For the sake of good folder organization, make a directory called **mhm_work** and subdirectory inside of it called **installation**. Following are the commands (in grey boxes) to be typed in the Linux terminal (hit enter after each line):

    ```bash
    mkdir mhm_work     # makes a directory called "mhm_work"
    cd mhm_work        # enters the newly made directory "mhm_work"
    mkdir installation # makes a directory called "installation"
    cd installation    # enters the newly made directory "installation"
    ```

    Now you are inside the **installation** directory you just made. All the installation work, including mHM installation, will be made here.

2.  In the Linux terminal, type or copy paste the following and press enter:

    ```bash
    wget --no-check-certificate https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
    ```

    Note: If the above web page doesn't work, go to [this website](https://docs.conda.io/en/latest/miniconda.html) in your Windows browser and copy download the link for the latest **Linux Installer** for miniconda. Replace the above web page address with this link.

    To check whether this step worked, type `ls` to see the folder content of current directory. You should be able to see a file with .sh extension. This is the Linux installation script for Miniconda. Miniconda, or just Conda in short, will help in warp-speed installation of mHM in your Linux OS.

    Hint: **wget** is the command that you can use to perform download directly from the terminal.

3.  Now install Conda from the installer script by typing the following in the terminal:

    ```bash
    bash Miniconda3-latest-Linux-x86_64.sh
    ```

    If everything goes well, you should be able to see a folder named **miniconda** in your working folder. View it using **ls** as done previously. Great, Conda installation is done.

4.  Now that you have Conda to help you, create a Conda environment with the following set of commands:

    ```bash
    conda create -y --prefix ./mhm_env
    conda activate ./mhm_env
    conda config --add channels conda-forge
    conda config --set channel_priority strict
    ```

    You just created the Conda environment called **mhm_env** in the current folder which has compilation of all the dependencies for mHM. If you have completed this step, you are just one step away from running mHM.

## Installing the latest release

In order to install the latest release of mHM, just type:

```bash
conda install -y mhm
```

Now you can run `mhm` by simply calling the command:

```bash
mhm
```

## Installing the latest development version

1.  mHM is located in a system called **git** over the Internet. Downloading mHM from git is called cloning. In order to clone mHM, type the following command in the terminal in the **installation** folder:

    ```bash
    git clone https://git.ufz.de/mhm/mhm.git
    ```

    Once the cloning is done, you will notice a folder named **mhm** in the installation folder. Great, you successfully cloned the mHM's git repository!

    Note: In case your Linux doesn't have git, you can install it using conda by typing

    ```bash
    conda install -y git
    ```

2.  Now that you have a copy of mHM repository as well as the Conda environment set for mHM, finally its time to install mHM. Go inside the mhm folder and compile mHM by typing the following commands:

    ```bash
    conda install -y cmake make fortran-compiler netcdf-fortran
    cd mhm                    # enters the directory "mhm"
    source CI-scripts/compile # runs the compilation for mhm
    ```

    If everything goes well, you should reach to the 100% progress point and see the message **Built Target mhm**. Now there would be a new file called **mhm** in the folder. This implies you just compiled the executable file of the mesoscale hydrological model, mhm. Congratulations, you have installed mHM!

3.  To run mhm, type the following command in the Linux terminal:

    ```bash
    ./mhm
    ```

    This runs the test domains that is provided with mhm repository as example/ reference. In order to run your own model domain and setup, refer to the **Getting started** and **Data Preparation for mHM** sections of the [mHM user manual](https://mhm.pages.ufz.de/mhm/latest/).

## Final remarks

Note: mHM needs the Conda environment **mhm_env** to run. However, this environment is specific to mHM and not needed at other times. A good practice would thus be to activate and deactivate the Conda environment before and after mHM needs to be used, respectively, by typing the following in the terminal from the **installation** folder:

```bash
conda activate ./mhm_env # activates the Conda environment
conda deactivate         # deactivates the active (i.e. Conda) environment
```
