# Document Profile : Scenario Test

| | |
| ------------: | ------------ |
| **Name**  | Scenario Test / Prueba de escenario / 시나리오 테스트 / シナリオテスト |
| **Path**  | %TESTDRIVE_PROFILE%/Common/ScenarioTest  |
| **Language**  | English / español / 한국어 / 日本語  |
| **Profile View** | System :fa-angle-right: Analysis :fa-angle-right: Scenario Test  |
 

시나리오 테스트는 프로그램을 실행하는 테스트 테스크를 점수화하여 관리하는 동적 스코어 보드입니다.
테스트를 체계적으로 관리하고 객관적인 점수화하여 프로젝트가 항상 오류가 발생하지 않도록 도와주며 테스트의 경과 시간을 표시합니다.

시나리오 테스트는 %PROJECT%Program/ScenarioTest 위치에 원하는 폴더를 생성 후 그 폴더 안에 Scenario.ini 를 기술하는 것으로 시작됩니다.

 

공개 테스트 프로젝트인 Meitner 를 예를 들어 설명하면, 위와 같이 Sample 이란 폴더를 생성하고 그 아래 Scenario.ini 를
아래와 같이 기술하였습니다. (ScenarioTest 폴더 안에 여러 폴더를 생성하여 별도의 테스트 그룹을 생성할 수 있습니다.)

```ini
[Scenario]
NAME=Test Samples
PROGRAM=Testbench_ScenarioTest.exe
PARAMETERS=%s
FILES=*.txt
PREFIX=<a href='open:%TESTDRIVE_DIR%bin/notepad/unicode/notepad++.exe?$(ROOT_PATH)$(FILE_PATH)/$(FILE_NAME)'><img src='edit.png' border='0'/></a>
ERROR_STRING=Status :@E:0;done!@E:0;*E :@E:-3
```

NAME 은 테스트 그룹 이름입니다.
PROGRAM 현재 폴더 및 하위 폴더에서 검색된 파일들을 실행하기 위한 실행 파일입니다.
각 검색된 파일은 PARAMETERS 에 기술된 내용을 바탕으로 클릭시 실행되게 됩니다.

위의 경우 %PROJECT%Program 폴더에 저장된 Testbench_ScenarioTest.exe 파일을 각 아래와 같이 검색된 파일 경로를 넣어 실행하게 됩니다. 


 
FILES 는 검색할 파일을 기술합니다. 여기서는 모든 txt 확장자를 가지는 파일 "*.txt" 로 기술되고 있습니다.
실제 검색된 파일은 위의 그림과 같습니다. 각 1-? 에 해당하는 .txt 파일을 클릭하면 개별적으로 테스트가 실행되고,
"1. Test Samples" 와 같이 회색 음영의 그룹명을 클릭하면 해당 그룹내의 모든 테스트들을 순차적으로 실행하게 됩니다.

PREFIX 는 각 그룹내 테스트 리스트 문자열 앞에 추가할 html 문법입니다. 여기서 별도의 환경변수를 사용할 수 있는데 아래와 같은
의미로 대체됩니다.
```
$(ROOT_PATH)                 : "%PROJECT%Program\" 경로
$(FILE_PATH)                 : ROOT_PATH 를 제외한 파일까지의 경로
$(FILE_NAME)                 : 파일의 이름
```
따라서 파일의 전체 경로는 "$(ROOT_PATH)$(FILE_PATH)/$(FILE_NAME)" 가 됩니다.
여기서 사용된 이미지는 "%TESTDRIVE_PROFILE%Common/media" 에서 검색됩니다.

위에서는 기술되지 않았지만 POSTFIX 아이템도 PREFIX와 동일한 기능을 하지만 테스트 리스트 문자열 뒤쪽에 붙는 html 문법을
나타냅니다.

ERROR_STRING 은 테스트의 성공/실패/크래쉬 ... 등을 구분하기 위해 프로그램이 출력하는 문장으로부터 테스트 결과를 구분하기 위한
ID를 기술합니다. 하나의 문자열에 대한 기술 방법은 아래와 같으며, 여러 검출 문자열을 사용하려면 ';' 로 구분합니다.

검출문자열@E:구분번호

프로그램 실행 중 검출문자열이 검색되면 구분번호를 참조하여 마킹합니다.
구분번호는 1 이상일 경우 [성공]으로 간주하고,
0은 중립, -1 은 [실패] 로 -2 이하는 절대값을 취하여 아래 테이블의 Value 에 해당하는 Status를
참조하여 테스트 결과를 지정합니다.


| ID | Value | Status | Default string |
| --- | :---: | :---: | --- |
| TEST_STATUS_NOT_TESTED | 0 | [테스트 필요] | "Status : Not tested" |
| TEST_STATUS_RUN_PASSED | 1 | [성공] | "Status : Process is passed" |
| TEST_STATUS_RUN_FAILED | 2 | [실패] | "Status : Process is failed" |
| TEST_STATUS_RUN_CRACHED | 3 | [크래쉬] | "Status : Program is crashed" |
| TEST_STATUS_COMPILE_FAILED | 4 | [컴파일 실패] | "Status : Compilation is failed" |
| TEST_STATUS_LINK_FAILED | 5 | [링크 실패] | "Status : Linking is failed" |
| TEST_STATUS_FILE_NOT_FOUND | 6 | [파일없음] | "Status : File is not found" |


ERROR_STRING을 기술하지 않고도 위의 "Default string" 기본 설정된 문장을 찍는 것만으도 테스트가 구분 가능합니다.

C 소스에서는 "%TESTDRIVE_PROFILE%Common/include/ScenarioTest.inl" 를 include 하여 쉽게 위의 ID 에 해당하는
Default string 을 출력 가능합니다.
```cpp
typedef enum{
	TEST_STATUS_NOT_TESTED,
	TEST_STATUS_RUN_PASSED,
	TEST_STATUS_RUN_FAILED,
	TEST_STATUS_RUN_CRACHED,
	TEST_STATUS_COMPILE_FAILED,
	TEST_STATUS_LINK_FAILED,
	TEST_STATUS_FILE_NOT_FOUND,
	TEST_STATUS_SCORE,
	TEST_STATUS_SIZE
}TEST_STATUS;

LPCTSTR	__sTestStatusList[TEST_STATUS_SIZE] = {
	_T("Status : Not tested"),
	_T("Status : Process is passed"),
	_T("Status : Process is failed"),
	_T("Status : Program is crashed"),
	_T("Status : Compilation is failed"),
	_T("Status : Linking is failed"),
	_T("Status : File is not found"),
	_T("Status : Score = %lf%%"),
};

void TestResultOut(TEST_STATUS id){
	if(id<TEST_STATUS_SCORE){
		printf("%s\n", __sTestStatusList[id]);
		fflush(stdout);
	}
}
```

'TEST_STATUS_SCORE' 를 제외하고, TestResultOut 함수를 사용하여, 테스트 결과를 시나리오 테스트에 알려줍니다.
실제 여기서 사용한 Testbench_ScenarioTest.exe 소스
"%PROJECT%Application/Example/Testbench/ScenarioTest/main.cpp" 를 보면 쉽게 알 수 있습니다.

```cpp
#include "STDInterface.h"
#include "ScenarioTest.inl"

int main(int argc, const char* argv[])
{
	if(argc != 2) {
		printf("*E : No argument...\n");
		return 0;
	}

	{
		int id	= atoi(argv[1]);
		FILE* fp = fopen(argv[1], "rt");

		if(fp) {
			int id;
			fscanf(fp, "%d", &id);

			if(id >= 0 && id < TEST_STATUS_SIZE)
				TestResultOut((TEST_STATUS)id);
			else
				printf("*E : invalid argument.\n");

			fclose(fp);
		} else {
			TestResultOut(TEST_STATUS_FILE_NOT_FOUND);
		}
	}

	return 0;
}
```

argv 로부터 테스트 파일명을 입력받아 읽고 fscanf 로 파일에 담겨진 정수값을 읽어
TestResultOut 을 통해 상태를 출력하는 예제입니다.
"1-8. /Sample/test1/7.txt" 의 경우 value 범위를 벗어나 아래 명령을 수행하게 되는데....

`printf("*E : invalid argument.\n");`


Scenario.ini 에 기술된 ERROR_STRING 의 기술된 문장 중...

`ERROR_STRING=Status :@E:0;done!@E:0;*E :@E:-3`


"*E :" 검출문자열에 해당하는 -3 이 인식되어 절대값 3 에 해당하는 TEST_STATUS_RUN_CRACHED 로 인식하여
[크래쉬] 상태가 되는 예제입니다.


경우에 따라 테스트의 결과를 성공/실패로 나누기 어려운 일이 있을 경우 아래와 같이 테스트를 점수화 시켜 저장할 수 있습니다.

 
위 예제는 %PROJECT%Program/ScenarioTest/Vectors/Scenario.ini 에서 아래와 같이 SCORE_FORMAT 을 제거하고 실행된 화면입니다.
SCORE_FORMAT 에 대해서는 뒤에 자세히 다시 설명합니다.
```ini
[Scenario]
NAME=Test Vectors
PROGRAM=Testbench_ScenarioVector.exe
PARAMETERS=%s
FILES=*.txt
PREFIX=<a href='open:%TESTDRIVE_DIR%bin/notepad/unicode/notepad++.exe?$(ROOT_PATH)$(FILE_PATH)/$(FILE_NAME)'><img src='edit.png' border='0'/></a>
ERROR_STRING=Status :@E:0;done!@E:0;*E :@E:-3
;SCORE_FORMAT=%.1lf point;0;90
```

'Meitner' 기본 예제상의 테스트 벡터들은 %PROJECT%Program/ScenarioTest/Vectors 폴더에 기술되어 있으며,

실제 여기서 사용한 Testbench_ScenarioVector.exe 소스
"%PROJECT%Application/Example/Testbench/ScenarioVector/main.cpp" 를 보면 쉽게 알 수 있습니다.

```cpp
#include "STDInterface.h"
#include "ScenarioTest.inl"

int main(int argc, const char* argv[])
{
	if(argc != 2) {
		printf("*E : No argument...\n");
		return 0;
	}

	{
		FILE* fp = fopen(argv[1], "rt");

		if(fp) {
			double score;
			fscanf(fp, "%lf", &score);
			fclose(fp);
			TestScoreOut(score);
		} else {
			TestResultOut(TEST_STATUS_FILE_NOT_FOUND);
		}
	}

	return 0;
}
```

TestScoreOut 함수를 통해 테스트 점수를 출력할 수 있습니다.
이 함수는 ScenarioTest.inl 에 아래와 같이 정의 되어 있습니다.

```cpp
void TestScoreOut(double fScore){
	printf(__sTestStatusList[TEST_STATUS_SCORE], fScore);
	printf("\n");
	fflush(stdout);
}
```

기본적으로 최소값 0, 최대값 100을 가지는 % 표현됩니다.
하지만 테스트 종류에 따라 아래처럼 표현 방법이 달리질 수 있습니다.

 
이 것은 포멧을 "%.1lf point", 최소 값 0, 최대 값 90 으로 ScenarioTest.ini 에 아래처럼 설정한 것입니다.
```
SCORE_FORMAT=%.1lf point;0;90

SCORE_FORMAT의 표기 방식은 다음과 같습니다.

SCORE_FORMAT=문자열_포멧;최소값;최대값
```

최소값 최대값은 생략 가능하며, 이때에는 기본값 최소 0, 최대 100 으로 지정됩니다.
최소값과 최대값을 동일하게 지정할 경우에는 해당 그룹은 보이지 않고 오류 메시지를 출력하게 됩니다.


기본적으로 스코어 결과는 '[성공]' 으로 간주하며 전체 성공 개수가 +1 됩니다.
'전체 성공 %' 에서만 'TestResultOut(TEST_STATUS_RUN_PASSED)' 의 성공과 차이를 가지게 되는데,
TEST_STATUS_RUN_PASSED 는 100% '개별 스코어 %' 로 취급하고
스코어 결과인 경우는 아래와 같이 '개별 스코어 %' 를 계산하여 적용됩니다.

'개별 스코어 %' = ((스코어 - 최소값) * 100) / (최대값 - 최소값)
'전체 성공 %' = '개별 스코어 %' 총 합산 / 총 테스트 수

물론 일반적인 성공/실패 시나리오와 스코어 테스트는 동일한 그룹내에서 혼용하여 사용할 수도 있습니다.



그룹내의 모든 테스트들을 종합하여 아래와 같이 그룹 테스트 스코어를 알려줍니다.

 
그룹명의 왼쪽에 폴더 아이콘 클릭시 해당 그룹의 폴더 위치를 탐색하도록 자동으로 생성됩니다.

모든 그룹의 총 결과는 테이블의 맨 아래에 아래와 같이 스코어 결과를 출력하게 됩니다.

 
시나리오 테스트는 단순히 프로그램의 실행 결과를 printf 로 출력된 문자열로 구분하여 테스트 스코어를 단순히 계산하지만,
일괄적인 테스트 환경을 구축하고 한눈에 테스트 진행 여부를 파악하도록 합니다.

### [:fa-arrow-left: Back](?top.md)
