# Code Analysis (Document)

| | |
| :------------: | ------------ |
| **Name**  | Code Analysis / Análisis de código / 코드 분석 / コード分析 |
| **Path**  | %TESTDRIVE_PROFILE%/Common/CodeAnalysis  |
| **Language**  | English / español / 한국어 / 日本語  |
| **Profile View** | System > Analysis > Code Analysis  |



이 프로파일은 프로젝트에서 지정한 폴더의 C/C++ 소스 검사를 CppCheck를 통해 시행하는 방식입니다.
 
Meitner/Profiles/CodeAnalysis.ini 파일에 예제로 검사 대상을 지정하는 방식이 기술되어 있습니다.
 예) CodeAnalysis.ini
```ini
[CodeAnalysis]
NAME_0 = SystemHAL
PATH_0 = %PROJECT%System\SystemHAL
NAME_1 = Simulation_SimHDL
PATH_1 = %PROJECT%System\SubSystems\Simulation\SimHDL
NAME_2 = Simulation_SimSystem
PATH_2 = %PROJECT%System\SubSystems\Simulation\SimSystem
NAME_3 = DDK
PATH_3 = %PROJECT%Application\DDK\source
NAME_4 = Example
PATH_4 = %PROJECT%Application\Example\Testbench
```
위와 같이 NAME_# 과 PATH_# 에 프로젝트의 소스 경로와 분석 대상 이름을 지정합니다.
CodeAnalysis 에서는 지정한 경로의 모든 하위 폴더에 들어있는 C/C++ 소스에 대해서 CppCheck 를 시행합니다.
이 때 오류나 경고가 있으면 붉은 색으로 메시지가 출력 되며 클릭을 통해 직접 오류 위치에서 바로 수정할 수 있습니다.
(Meitner 프로젝트는 일부러 예시를 위해 위와 같이 오류를 수정하지 않았습니다.)
 
위의 예에서 새로운 소스 경로를 추가하려면 NAME_5 와 PATH_5 와 같이 연속된 번호를 이름에 붙여 원하는 경로와 이름을 추가하시면 됩니다.
 
소스가 많으면 한눈에 확인하기 힘들기 때문에 통과된 소스들에 대해서는 별도로 기록을 남기지 않습니다.
모든 기록을 보려면 "속성 뷰" 에서 '로그 생략' 을 False로 지정하면 모든 로그 기록을 볼 수 있습니다.
(기본으로 'True' 로 설정되어 있습니다.)

이 프로파일은 TestDrive 프로그램이 실행될 당시 SystemManager 프로파일에 의해 매 월요일 1회 자동으로 실행되고 있으며,
이 도큐먼트에서 오류를 라고 표사하더라도 항상 소스가 문제가 있다는 뜻은 아니며, 소스의 완결성에 오류가 있다는 것입니다.
따라서 가능한 여기서 발견되는 오류나 경고를 살펴보는 주의가 필요합니다.

### [:fa-arrow-left: Back](?top.md)
