# Tutorial : C/C++ Project

#### Quick approach) "Project creation :fa-arrow-right: build :fa-arrow-right: run" series of an example
```bash
C:\Project>create_project c Test
Done.
C:\Project>cd Test

C:\Project\Test>ls
Makefile  main.cpp

C:\Project\Test>make
- Compiling... : main.cpp

*** Build execution file ***
x86_64-w64-mingw32-g++  -o Test.exe ./main.o   -LC:/Project/profiles/Common/lib -LC:/Project/TestDrive/bin/msys64/usr/lib -lstdc++ -lpthread -lm

C:\Project\Test>ls
Makefile  Test.exe  main.cpp  main.d  main.o

C:\Project\Test>Test.exe
Hello~!
```

## 1. Project creation
Create a project ('Project_Name') by entering a command in the command window as shown below.
```bash
create_project c [Project_Name]
```
At this moment, The default project will be created with the specified 'Project_Name' name.
> :fa-send-o:Tip : By using "create_project util [Project_Name]" instead of "create_project c [Project_Name]", you can use the extended 'util_framework' functionality.

## 2. Build
Below codes are the 'Makefile' of the created project.
```bash
#-------------------------------------------------
# Project
#-------------------------------------------------
SRC_PATH        := .
TARGETPATH      := .
TARGETNAME      := Test

SRCS            := \
	$(wildcard $(SRC_PATH)/*.cpp) \
	$(wildcard $(SRC_PATH)/*.c)
	
INC             := -I.
LIBDIR          :=
CDEFS           :=
EXTRA_CFLAGS    := -O0 -std=c++17 -DDEBUG -g

#-------------------------------------------------
# common document config
#-------------------------------------------------
BUILD_TARGET    = $(TARGET_EXE)
include $(TESTDRIVE_PROFILE)Common/include/config.mk
```
The variable properties specified here are as follows.

| Variable | Description |
| ------------ | ------------ |
| SRC_PATH  | Default source path |
| TARGETPATH  | Result target path |
| TARGETNAME  | Result Target name |
| SRCS | Source list |
| INC | User defined include path list  |
| LIBDIR | User defined library/path list |
| CDEFS | User definition list |
| EXTRA_CFLAGS | Extra definition list (If not specified, default EXTRA_CFLAGS is "-O3 -std=c++17".)  |
| BUILD_TARGET | Build result type<br>- $(TARGET_EXE) : Execution file (.exe)<br>- $(TARGET_SO) : Dynamic library (.so / .dll)<br>- $(TARGET_A) : Static library (.a)<br>- $(TARGET_SO_A) : Dynamic & Static library |
| BUILD_VERSION | set '1' to generate automatic version header.<br>'make version_minor' command will update minor version.<br>'make version_major' command will update major version.  |

Build command is `make`.

## 2. Debugging
For debugging, use the eclipse IDE environment as follows, and execute `.eclipse` in the created project.
```dos
C:\Project\Test>ls -al
total 32
drwxr-xr-x 1 USER None     0 Apr 15 14:41 .
drwxr-xr-x 1 USER None     0 Apr 15 12:32 ..
-rw-r--r-- 1 USER None    53 Apr 15 12:32 .eclipse.bat
-rw-r--r-- 1 USER None 13154 Apr 15 12:32 .cproject
-rw-r--r-- 1 USER None    33 Apr 15 12:32 .inception
-rw-r--r-- 1 USER None  1280 Apr 15 12:32 .project
-rw-r--r-- 1 USER None   530 Apr 15 12:32 Makefile
-rw-r--r-- 1 USER None   103 Apr 15 12:32 main.cpp
```
[Run Screen] 
![c_project](img/project_c.jpg)

> :fa-send-o:Tip : It is recommend that you update the index by running the following at first. "Project Explorer :fa-arrow-right: mouse left button :fa-arrow-right: index :fa-arrow-right: Rebuild".

1) Press `CRTL + B` to run the build.
2) Press F5 in "Project Explorer" to confirm that "Test.exe" is added.
3) Press `F11` and select "Local C/C++ Application", switch to Debug mode, and debug.

> :fa-send-o:Tip : After debugging, make sure the program is closed before rebuilding.
 Build results cannot be overwritten without exiting the program.

## 3. Encryption and Licensing Management

...(under construction...)

### [:fa-arrow-left: Back](?top.md)
