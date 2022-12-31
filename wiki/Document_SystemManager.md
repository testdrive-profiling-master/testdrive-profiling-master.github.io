# Document Profile : System Manager

| | |
| ------------: | ------------ |
| **Name**  | System / Sistema / 시스템 / システム |
| **Path**  | %TESTDRIVE_PROFILE%/Common/SystemManager  |
| **Language**  | English / español / 한국어 / 日本語  |
| **Profile View** | (auto started.)  |

sw/hw 개발 환경, 컴파일 자동화, 코드 lint, 시뮬레이션 파형 제어, 코딩 스타일 제어 등을 관리하는 도큐먼트입니다.
속성 뷰에서 "시스템" 프로파일

## 1. 하위 시스템

%PROJECT%Profiles/Config.ini 에 다음과 같이 기술됩니다.

```
[System]
SubSystem[0]	= SimSystem.dll@Simulation
SubSystem[1]	= EmulSystem.dll@Emulation
SubSystem[2]	= DriverSystem.dll@Driver
```

위와 같이 System 섹션에 다음과 같이 아이템을 기술하여 하위 시스템 리스트를 추가합니다.

```
SubSystem[N]    = 파일명@기술명
```

N 은 0, 1, 2, 3.... 순서로 리스트 추가
파일명은 %PROJECT%Program/ 위치의 존재하는 하위 시스템 dll 파일명
기술명은 속성 뷰 상에 대체 이름

하위 시스템은 HAL 다음의 하위 레이어로 %PROJECT%Application/include/ddk/SystemHAL.h 의
IMemoryImp, ISystemImp 인터페이스를 구현하고 CreateSystemImplementationFunction export 함수를
통하여 ISystemImp 객체를 반환하는 것으로 구현된다.

선택된 하위시스템은 DDK 를 사용하는 최상위 응용프로그램에 의해 접근이 가능하게 됩니다.
여러 하위 시스템은 TestDrive 에 의해 선택되어지며, 응용프로그램은 하위 시스템이 HAL 에 의해 추상화되므로
그 구현에 무관하게 동일하게 프로그램이 작성 가능합니다.

응용프로그램에서 하위 시스템을 구별하기 위해서는 DDK의 GetSystemDescription 함수를 통해 하위 시스템에서
지정한 문자열을 읽어 확인는 방법이 있습니다.
동일 환경에서 여러 하위 시스템의 동작이 서로 다를 경우 위 함수를 통해 응용프로그램의 오동작을 사전에
차단할 수 있습니다.

Meitner 예제상 각 하위 시스템은 %PROJECT%System/SubSystems/ 의 하위 폴더에 기술되어 있다.


## 2. 빌드 자동화

%PROJECT%Profiles/Config.ini 에 다음과 같이 기술됩니다.
```sh
[Build Automation]
Item[0].title			= Processor
Item[0].extensions		= v;sv;vh;h;c;cpp
Item[0].inception		= %PROJECT%Profiles\code_inception.txt
Item[0].link_item		= 1
Item[0].watch_path		= System\HDL\
Item[0].search_subdir	= 1
Item[0].target			= top.sv
Item[0].argument		= -Mdir ../SubSystems/Simulation/verilator -prefix SimTop --top-module top

Item[1].title			= Simulation HDL module
Item[1].extensions		= h;c;cpp
Item[1].inception		= %PROJECT%Profiles\code_inception.txt
Item[1].error_string		=
Item[1].watch_path		= System\SubSystems\Simulation\SimHDL\
Item[1].search_subdir	= 1
Item[1].target			= make

Item[2].title			= Simulation sub-system
Item[2].extensions		= h;c;cpp
Item[2].inception		= %PROJECT%Profiles\code_inception.txt
Item[2].error_string		=
Item[2].watch_path		= System\SubSystems\Simulation\SimSystem\
Item[2].search_subdir	= 1
Item[2].target			= make
Item[2].argument		= -j

Item[3].title			= Emulation sub-system
Item[3].extensions		= h;c;cpp
Item[3].inception		= %PROJECT%Profiles\code_inception.txt
Item[3].error_string		=
Item[3].watch_path		= System\SubSystems\Emulation\
Item[3].search_subdir	= 1
Item[3].target			= make
Item[3].argument		= -j

Item[4].title			= HAL
Item[4].extensions		= h;c;cpp
Item[4].inception		= %PROJECT%Profiles\code_inception.txt
Item[4].error_string		=
Item[4].watch_path		= System\SystemHAL\
Item[4].search_subdir	= 1
Item[4].target			= make
Item[4].argument		= -j

Item[5].title			= DDK
Item[5].extensions		= h;c;cpp
Item[5].inception		= %PROJECT%Profiles\code_inception.txt
Item[5].error_string		=
Item[5].watch_path		= Application\DDK\source\
Item[5].search_subdir	= 1
Item[5].work_path		= Application\DDK\
Item[5].target			= make
Item[5].argument		= -j

Item[6].title			= API
Item[6].extensions		= h;c;cpp
Item[6].inception		= %PROJECT%Profiles\code_inception.txt
Item[6].error_string		=
Item[6].watch_path		= Application\API\source\
Item[6].search_subdir	= 1
Item[6].work_path		= Application\API\
Item[6].target			= make
Item[6].argument		= -j

Item[7].title			= Testbench
Item[7].extensions		= h;c;cpp
Item[7].inception		= %PROJECT%Profiles\code_inception.txt
Item[7].error_string		=
Item[7].watch_path		= Application\Example\Testbench\
Item[7].search_subdir	= 1
Item[7].target			= make
Item[7].argument		= -j
```

위와 같이 "Build Automation" 섹션에 다음과 같이 N 번째 아이템을 기술하여 자동화 빌드가 "True" 일 경우
해당 폴더의 프로젝트를 자동으로 빌드하도록 수행합니다.

```
Item[N].title			: 이름 지정
Item[N].extension		: 검색할 파일 확장자, 여러 파일 확장자를 지정할 경우 ';' 문자로 구분한다.
Item[N].inception		: 코드 인셉션(코드 첫 부분에 자동으로 삽입되는 문구) (사용하지 않을 시 비운다.)
					  예제의 %PROJECT%Profiles\code_inception.txt 을 보면 다음과 같이 구성되어 있다.

					  //=======================================================================__CHECK__
					  //__CHECK__ CloneX developments. Copyright 2013~__YEAR__. ALL RIGHTS RESERVED
					  //__CHECK__ Title : __TITLE__
					  //__CHECK__ Rev.  : __DATE__ (__AUTHOR__)
					  //=======================================================================__CHECK__

					  __CHECK__	: 실제 사
					  __TITLE__	: Item[N].title 에 지정한 이름으로 대체 기록된다.
					  __YEAR__	: 현재 연도로 대체 기록된다.
					  __DATE__	: 현재 날짜로 대체 기록된다.
					  __TIME__	: 현재 시간이 대체 기록된다.
					  __AUTHOR__	: '시스템' 속성 뷰의 개발자 서명으로 대체 기록된다.
					  __VERSION__	: 현재 버전으로 대체 기록된다. (지정 안됨.)

					  code inception 은 프로젝트의 상위 폴더에 ".inception" 파일의 아래와 같은 기술에 의해 대체된다.

					  각 항목은 생략할 수 있으며, 이 때에는 상위의 전역 설정에 따른다.

					  [INCEPTION]
					  inception="inception 파일 override"
					  author="저작자 이름 override"
					  title="타이틀 override"
					  version="버전 override"


Item[N].error_string		: 빌드 실행할 때 추가적으로 검출할 에러 문자열을 지정한다. (사용하지 않을 시 비운다.)
Item[N].link_item		: 비드가 끝나고 다음 연속으로 진행할 Item[N] 의 N을 지정한다. (사용하지 않을 시 비운다.)
Item[N].watch_path		: 파일 변경을 감시할 폴더 경로.
Item[N].search_subdir	: 하위 폴더 감시 여부. (1 or 0)
Item[N].target			: 빌드에 실행할 명령어. (verilog 빌드일 경우 특별히 대상 top 파일을 지정하는데, 실제 "8. 시뮬레이션 탑 디자인"으로 대체된다.)
Item[N].argument		: 빌드에 사용할 파라미터
```

자동화 빌드시 약 2초간 'CONTROL' 키를 누르면 자동화 빌드가 설정되어 있더라도 수행하지 않는다는 안내와 함께 빌드가 취소됩니다.

## 3. 디버그 정보

"2. 빌드 자동화"에 의해 빌드시 debug 빌드가 True 일 경우 GCC 옵션 "-g -O0" 을 사용하고,
False 일 경우 "-O3" 옵션을 사용한다.

디버그 정보를 False 로 설정할 경우 실행이 가장 빠른 최적화 빌드를 수행하지만, 하드웨어의 경우
이를 위해 파형을 관찰 할 수 없게 됩니다.

## 4. 코딩 A-스타일

C/C++/java 코딩 스타일을 지정하고, "2. 빌드 자동화"의 검색 대상이 저장될 때 자동으로
지정된 코딩 스타일로 변경한다.
상세 설명은 http://astyle.sourceforge.net/ 를 참조.
현재는 아래 옵션이 기본 값으로 지정되어 있다.
필요에 따라 변경한다.

```
--style=linux -T -n -k1 -L -xc -xn -xl -f -p -U -xe
```


## 5. 코딩 I-스타일

Verilog 코딩 스타일을 지정하고, "2. 빌드 자동화"의 verilog 검색 대상이 저장될 때 자동으로
지정된 코딩 스타일로 변경한다.
상세 설명은 https://code.google.com/p/istyle-verilog-formatter/ 를 참조.
현재는 아래 옵션이 기본 값으로 지정되어 있다.
필요에 따라 변경한다.

```
--style=kr -T4 -n
```


## 6. 엄격한 빌드

이 값이 True일 경우 "2. 빌드 자동화" 에서 감시 대상 파일이 변경될 때,
해당 파일이 C/C++ 일 경우 CppCheck, Verilog 의 경우는 Verilator Lint 를 통해
정적 lint 체크하여 경고나 오류가 있을 경우 빌드를 수행하지 않는다.
비록 빌드에 이상이 없는 프로젝트이더라도 보다 완성도 높은 코드를 유지하도록 한다.

verilog 의 경우 경고 출력 문구에서 이를 무시할 수 있는 방법도 같이 출력하므로 참고합니다.
verilog lint 에 대해 자세한 사항은 아래 verilator 메뉴얼을 참고합니다.

http://www.veripool.org/projects/verilator/wiki/Manual-verilator


## 7. 개발자 서명

"2. 빌드 자동화" 에서 감시 대상 파일이 변경될 때,
해당 파일의 코드 인셉션을 자동으로 삽입하여 파일의 출처 여부를 기록하면서
여기에 개발자 자신의 서명 정보가 삽입되게 된다.
이 때 적용할 개발자 서명을 지정하는 것이므로, 모든 개발자는 반드시 서명을 지정하도록 합니다.
지정하지 않을 경우 "Nobody" 로 기록된다.


## 8. 시뮬레이션 탑 디자인

이 속성은 "1. 하위 시스템" 이 'Simulation' 일 경우만 활성화 된다.
시뮬레이션 하위 시스템의 탑 디자인 파일을 지정한다.

직접 입력할 수도 있으나, %PROJECT%System/HDL/top_list.ini 에서 아래와 같이 미리 탑 디자인
리스트를 만들어 드롭다운 리스트에서 바로 선택 가능하게 됩니다.
S/W 팀과 같이 H/W 에 무관한 개발자들에게 편이한 H/W 모듈 선택하는 방법을 제공합니다.
디자인이 바뀌면 자동으로 H/W 빌드를 시행합니다.

```ini
[DESIGN]
DESIGN_0 = DUTs/meitner/top.sv
DESIGN_1 = DUTs/hdmi_controller/top.sv
DESIGN_2 = DUTs/FiFoTest/top.sv
DESIGN_3 = DUTs/AsyncFiFoTest/top.sv
DESIGN_4 = DUTs/processor_axi_wrapper/top.sv
```

 
간단히 DESIGN_N 아이템에 탑 디자인 파일 경로를 지정할 수 있다.
이 경로는 %PROJECT%System/HDL/ 에서 부터의 상대 경로로 지정한다.

참고로 사용되어지는 DPI 소스들은 %PROJECT%System/HDL/dpi/ 폴더 내의 모든 C/C++ 소스들이
자동으로 포함되며, 추가로 시뮬레이션 탑 디자인의 dpi_private 폴더가 추가적으로 포함됩니다.
위의 선택에서는 DUT/processor_axi_wrapper/dpi_private/ 폴더내의 C/C++ 소스들이 추가적으로
포함되게 됩니다. (ex. DUTs/hdmi_controller/dpi_private 폴더 참조.)


## 9. 시뮬레이션 정의

이 속성은 "1. 하위 시스템" 이 'Simulation' 일 경우만 활성화 된다.

직접 입력할 수도 있으나, %PROJECT%System/HDL/top_list.ini 에서 아래와 같이 작성된 정의를 사용할 수 있다.
시뮬레이션 정의가 변경되면 자동으로 H/W 빌드를 시행합니다.

```ini
[DEFINITION]
DEFINITION_0 = AA
DEFINITION_1 = BB@meitner
DEFINITION_2 = CC=1@hdmi_controller
```


아래와 같이 N 번째 아이템을 지정할 수 있다.

DEFINITION_N = 정의;정의...@한정자

여기서 여러 정의를 지정할 경우 ';' 문자로 구분하여 지정하며, "8. 시뮬레이션 탑 디자인" 의 종류에 따라 가능한
정의를 제한하려면 '@' 를 삽입하고 한정자를 지정한다.
한정자가 지정되면, 탑 디자인 이름에 한정자의 문자열이 있을 때에만 리스트 목록에 표시되고,
불가능한 탑 디자인에서 사용을 제한할 수 있다.


## 10. 웨이브 출력

이 속성은 "1. 하위 시스템" 이 'Simulation' 일 경우만 활성화 된다.
아래의 3가지 옵션을 선택할 수 있다.

None		: 웨이브 출력을 사용하지 않으며, 가장 빠른 속도록 보인다.
File output	: "11. 출력 파일명" 에서 지정된 파일로 웨이브 파형을 저장한다.
Interactive	: 웨이브 파형을 실시간으로 관찰한다.

"3. 디버그 정보" 가 True 일 경우에만 웨이브 출력을 선택할 수 있습니다.
'디버그 정보' 가 False 일 경우 이 속성은 None 으로 무시됩니다.


## 11. 출력 파일명

이 속성은 "1. 하위 시스템" 이 'Simulation' 일 경우만 활성화 된다.
"10. 웨이브 출력" 이 'File output' 로 지정되었을 경우 사용시 저장될 파형 파일 이름을 지정한다.
%PROJECT%Program/ 위치에서 상대 경로로 지정할 수 있다.


## 12. DO 파일명

DO 파일은 시뮬레이션 파형 동작시 GtkWave 에 초기에 자동으로 추가될 신호들을 지정한다.
간단하게 GtkWave 프로그램에서 수동으로 신호를 추가하고 'CTRL+S' 를 눌러 DO 파일로 저장 후,
다음 시뮬레이션 재시작시 자동으로 이 DO 파일을 참조하여 신호들을 복구시킨다.
이 속성은 "1. 하위 시스템" 이 'Simulation' 일 경우만 활성화 된다.
"10. 웨이브 출력" 에서 'Interactive' 로 설정 하여 실행하거나,
또는 프로파일 뷰에서 "System/View wave-form" 을 실행할 경우
사용할 do 파일을 지정한다.
GtkWave 에서 수정 후 저장될 수 있으며, %PROJECT%Program/ 위치에서 상대 경로로 지정한다.


## 13. 트레이스 시작 시간

이 속성은 "1. 하위 시스템" 이 'Simulation' 일 경우만 활성화 된다.
파형 기록이 활성화 될 ㎲ 단위의 시작 시간을 지정한다.
##### (GtkWave 에서 보이는 시간 단위는 ns 단위임을 유의한다.)
---
### [:fa-arrow-left: Back](?top.md)
