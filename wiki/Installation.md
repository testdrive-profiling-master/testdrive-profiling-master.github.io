# :fa-cogs: Installation

<iframe width="560" height="315" src="https://www.youtube.com/embed/hWhtecGd4l4" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

**Main Features**

* Alternative linux environment on windows with msys2/MinGW(UCRT version).
* S/W & H/W build & co-simulation supports.
* Dynamic documents support for CI(Continuous Integration) environment.

It can be installed only in **Windows 7/10/11** or higher 64bit OS environment.

## 1. Easy way
It help to install full feature of 'TestDrive Profiling Master'.
<i>**Download and Run**</i> below link.
> **[TestDrive Profiling Master Installer](../download/TestDrive_Profiling_Master.exe)** - (Automatic installation from git repositories.)
>( It is **strongly** recommended to install **[git](https://git-scm.com/downloads)** and **[tortoisegit](https://tortoisegit.org/download/)** separately. )

#### (1) Run 'TestDrive_Profiling_Master.exe'
```bash
******************************************************
****        TestDrive Automatic Installer.        ****
******************************************************

Set your install path (default[C:\Project]) :
```
Put your installation path or just leave it and press enter key. :)

## 2. Hard way, but more details

#### (1) Install from repositories.
First, install **[git](https://git-scm.com/downloads)** (**[tortoisegit](https://tortoisegit.org/download/)** optional) and enter the following through the command window in the appropriate folder.

```sh
cd /
mkdir Project
cd Project
git clone https://github.com/testdrive-profiling-master/release TestDrive
git clone https://github.com/testdrive-profiling-master/profiles Profiles
```

#### (2) Program environment registration
Enter as below to check if the program works normally.
During this process, environment variables are registered, and a reboot may be required.

```sh
cd TestDrive
TestDrive.exe
```
> If it does not run(Under the windows 7), the **visual studio redistributable package** must be installed first.
Execute `preinstallation/VC_redist.x64.exe` to complete the installation.

## 3. Install external tools
Now run the project file by typing the following.
In this process, the latest **linux build environment (msys2)** and **eclipse** are automatically installed.

``` sh
cd ../Profiles/Meitner
project.profile
```
> This may take about 10 minutes to 2 hours depending on your internet connection.
Depending on the anti-virus program, you may be asked to check the Internet connection, so if you have an anti-virus program installed, do not leave your desk and allow an Internet connection on every installation steps.

## 4. Confirmation of installation
In order to check whether the installation was successful, please reboot and check if the command can be executed normally.
`ls`
`gcc`
`make`
`%TESTDRIVE_DIR%bin/eclipse/eclipse`
`%TESTDRIVE_DIR%bin/notepad/notepad++`

:fa-thumbs-o-up: *Congratulations!*  All installations have been completed normally.

## 5. Trouble shooting
Contect to me : clonextop@gmail.com
### [:fa-arrow-left: Back](?top.md)
