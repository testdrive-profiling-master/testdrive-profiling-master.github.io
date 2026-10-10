# Project generator for TestDrive Profiling Master

You can create **<span style="color:red">hardware</span>** or **<span style="color:blue">software</span>** projects using this utility.

```bash
> create_project

Project generator for TestDrive Profiling Master.
Usage: create_project  [--help] type project_name

      --help                display this help and exit
  type                      Template project type
                            'testdrive'              : TestDrive project
                            'c', 'c++', 'cpp'        : C++(Pure) project
                            'util'                   : C++(Util) project
                            'lua'                    : C++(Lua) project
                            'webgui'                 : C++(WebGUI) project
                            'v', 'verilog'           : verilog project
                            'v_bare', 'verilog_bare' : bared verilog project
                            'verigen'                : verigen project
                            'docgen'                 : docgen project
                            'docgen_simplified'      : docgen simplified project
                            'svelte'                 : Svelte.js project
                            'uefi'                   : UEFI firmware project
                            'qemu'                   : QEMU project
  project_name              Project name


  ex) create_project cpp test

  * For some projects(testdrive, verilog, verigen, qemu...),
    you must run the TestDrive project at least once.
```
**command-line Example**
```bash
> create_project c test_c_project
```

Lists descriptions of the detailed projects that can be generated.

#### 1) testdrive
Create a minimal TestDrive project. The `testdrive` project must be executed at least once.

#### 2) c, c++, cpp
Create a C/C++ development project in the Eclipse IDE environment.

#### 3) util
Create a utility development project in the Eclipse IDE environment. This includes C, C++, and CPP projects. A custom `cstring` library and the like are being added.

#### 4) lua
Create a Lua development project in the Eclipse IDE environment. Include the `util` project. It includes an example of a Lua class, namespace and etc.

#### 5) webgui
Create a GUI development project in the Eclipse IDE environment. It includes the `util` project and is compilable on both Windows and Linux.

#### 6) v, verilog
This creates a Verilog development project within the Eclipse IDE environment, including standard Clock, Reset, Interrupt, and Busy signals.
The project is generated in the `System\HDL\DUTs` folder of the `testdrive` project environment.
therefore, the `testdrive` project must be run at least once beforehand.

#### 7) v_bare, verlog_bare
This creates a bare Verilog development project within the Eclipse IDE environment. It does not include standard Clock, Reset, Interrupt, or Busy signals; instead, these are defined as custom implementations within the source code.
The project is generated in the `System\HDL\DUTs` folder of the `testdrive` project environment.
therefore, the `testdrive` project must be run at least once beforehand.

#### 8) verigen
Create a Verilog development project in the Eclipse IDE environment using verigen.
Please refer to the [verigen](../download/verigen_userguide.pdf) document for detailed instructions.
The project is generated in the `System\HDL\DUTs` folder of the `testdrive` project environment.
therefore, the `testdrive` project must be run at least once beforehand.

#### 9) docgen
Create a document development project that utilizes `docgen`. The top-level project file begins with a `Lua` script.
For detailed information, please refer to the [docgen](?tutorial_docgen.md) documentation.

#### 10) docgen_simplified
Create a document development project that utilizes docgen. The top-level project file begins with a `markdown` script.
For detailed information, please refer to the [docgen](?tutorial_docgen.md) documentation.

#### 11) uefi
Create a UEFI development project in an Eclipse IDE environment that utilizes the EDK2 compiler.
An initial EDK2 installation is performed during the first build, taking approximately 20 minutes.

#### 12) qemu
This sets up a QEMU project capable of running virtual devices.
It builds QEMU once upon initial execution and checks for updates on a weekly basis, allowing for a rebuild if updates are available.
The build process takes approximately 30 minutes.
For detailed information, please refer to the [QEMU(virtual machine) project tutorial](?tutorial_qemu.md) documentation.

### [:fa-arrow-left: Back](?top.md)