# Tutorial : C/C++ Project

#### Quick approach) "프로젝트 생성 :fa-arrow-right: 빌드 :fa-arrow-right: 실행" 일련의 과정 예시
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

## 1. 프로젝트 생성
아래와 같이 커맨드 창에서 명령어를 입력하여 프로젝트('Project_Name')를 생성합니다.
```bash
create_project c [Project_Name]
```
이 때 지정된 'Project_Name' 이름으로 기본 프로젝트가 생성됩니다.

## 2. 빌드
아래는 생성된 프로젝트의 Makefile 입니다.
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
EXTRA_CFLAGS    := -O0 -std=c++11 -DDEBUG -g

#-------------------------------------------------
# common document config
#-------------------------------------------------
BUILD_TARGET    = $(TARGET_EXE)
include $(TESTDRIVE_PROFILE)Common/include/config.mk
```
여기서 지정하는 변수 속성은 아래와 같습니다.

| Variable | Description |
| ------------ | ------------ |
| SRC_PATH  | 소스 기본 위치 |
| TARGETPATH  | 결과 저장 위치 |
| TARGETNAME  | 결과 이름 |
| SRCS | 소스 리스트 |
| INC | 추가 include 리스트  |
| LIBDIR | 추가 library 리스트 |
| CDEFS | 기본 정의 리스트 |
| EXTRA_CFLAGS | 추가 정의 리스트 (지정되지 않을 경우 "-O3 -std=c++11" 로 지정됩니다.)  |
| BUILD_TARGET | 빌드 결과 종류<br>- $(TARGET_EXE) : 실행 파일 (.exe)<br>- $(TARGET_SO) : 동적 라이브러리 (.so / .dll)<br>- $(TARGET_A) : 정적 라이브러리 (.a)<br>- $(TARGET_SO_A) : 동적 라이브러리 + 참조 라이브러리 |

빌드는 `make` 를 이용합니다.

## 2. 디버깅
디버깅을 위해서는 아래와 같이 eclipse IDE 환경을 이용하며, 생성된 프로젝트 안에서 `.Project.bat` 를 실행합니다.
```dos
C:\Project\Test>ls -al
total 32
drwxr-xr-x 1 USER 없음     0 Apr 15 14:41 .
drwxr-xr-x 1 USER 없음     0 Apr 15 12:32 ..
-rw-r--r-- 1 USER 없음    53 Apr 15 12:32 .Project.bat
-rw-r--r-- 1 USER 없음 13154 Apr 15 12:32 .cproject
-rw-r--r-- 1 USER 없음    33 Apr 15 12:32 .inception
-rw-r--r-- 1 USER 없음  1280 Apr 15 12:32 .project
-rw-r--r-- 1 USER 없음   530 Apr 15 12:32 Makefile
-rw-r--r-- 1 USER 없음   103 Apr 15 12:32 main.cpp
```
[실행화면] 
![c_project](img/project_c.jpg)

> 팁 : "Project Explorer > mouse left button > index > Rebuild" 를 실행하여 초기에 index 갱신을 하는 것이 좋습니다.

1) `CRTL + B` 를 눌러 빌드를 실행합니다.
2) "Project Explorer" 에서 F5 를 눌러 "Test.exe" 가 추가됨을 확인합니다.
3) `F11` 를 눌러 "Local C/C++ Application" 을 선택 후 Debug 모드로 스위치 한 후 디버깅 할 수 있습니다.

> 디버깅 후 다시 빌드 하기 전에 프로그램이 종료 되었는지 확인 하세요.
 프로그램이 종료되지 않은 상태에서는 빌드 결과를 덮어 쓸 수 없습니다.

## 3. 암호화 및 라이센싱 관리

...(under construction...)

### [:fa-arrow-left: Back](?top.md)
