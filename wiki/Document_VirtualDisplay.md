# Virtual Display (Document)

| | |
| :------------: | ------------ |
| **Name**  | Display / Monitor / 디스플레이 / ディスプレー |
| **Path**  | %TESTDRIVE_PROFILE%/Common/VirtualDisplay  |
| **Language**  | English / español / 한국어 / 日本語  |
| **Profile View** | System/Display  |
 

가상의 디스플레이 도큐먼트를 S/W 응용프로그램과 H/W 응용 프로그램에 접근할 수 있도록 한다.

디스플레이 도큐먼트는 프로젝트에서 생성한 메모리 모델의 특정 값을 모니터링하여,
해당 값이 변경될 경우 자동으로 디스플레이에 영향을 주게 된다.

먼저 Meitner 예제 프로젝트에서 "project.profile" 파일의 메모리 모델 생성 부분을 살펴본다.
```
// Create memory
memory.create		0x8000000, "Meitner"        // System memory : 128MB
memory.create		0x10000, "Meitner_Display"  // Display configuration memory & etc..(register map) : 64KB
```
위에서 두 메모리 모델을 생성하고 있으며, 실제 TestDrive 출력창에서 아래와 같이 로그가 출력된다.

메모리 모델 생성 : 'Meitner' : 134217728 Bytes (128.0MB)
메모리 모델 생성 : 'Meitner_Display' : 65536 Bytes (64.0KB)
처음 생성한 Meitner 메모리 모델은 시스템 메모리로 사용하며, Display 가 출력하는 프레임 버퍼는 이곳에서 참조합니다.
디스플레이의 높이, 너비, 색상 포멧등의 속성은 시스템 메모리의 이름+"_Display" 에 해당하는 메모리 모델의 Config 영역을
수정함으로써 변경 가능합니다.


### 메모리 모델에 대해서.

먼저 메모리 모델에 대해서 .profile 문법을 통해 메모리 모델을 생성하였을 때, 지정한 메모리 크기만큼의  thunk 메모리와
Config 메모리 영역 2개가 할당되고, 여러 프로세스가 같이 공유될 수 있도록 다음과 같은 memory mapped 메모리가 설정된다.

[4Kbyte Config 메모리] + [사용자 할당 메모리]

사용 참조 소스 :
`%TESTDRIVE_DIR%include/TestDriver.h`
`%TESTDRIVE_DIR%include/TestDriver.inl`

이렇게 할당된 메모리의 Config 부분은 제공되는 함수 GetConfig() 함수를 통해 전달 받을 수 있으며, 아래와 같은
구조체를 가진다.
```cpp
typedef struct{
	DWORD	dwMemorySize;
	DWORD	UserConfig[1023];
}TESTDRIVE_CONFIG;
```

여기에서 디스플레이 Config 메모리는 사용자가 지정한 메모리 크기(dwMemorySize : Byte 단위) 를 제외하고 사용자가 재정의 할 수 있다.

디스플레이 도큐먼트의 Config 는 시스템 메모리의 이름+"_Display" 메모리 모델의 Config 영역을 통해 제어하는데,
Meitner 예제에서는 "Meitner_Display" 메모리 모델이 해당한다.
이곳 Config 영역의 구조체는 아래 파일에 기술되어 있다.

`%TESTDRIVE_PROFILE%Common/include/VirtualDisplayConfig.h`
```cpp
typedef struct{
	DWORD			dwAddress;		// Offset address
	volatile BOOL		bUpdate;			// Update screen
}DisplayBuffer;

typedef struct{
	const DWORD		dwMemorySize;		// Allocated memory byte size (do not fix it.)
	HWND			hWndHandle;		// Virtual Display document window handle (64bit is not support.)
	int				iWidth;			// width
	int				iHeight;			// height
	DISPLAY_FORMAT	ColorFormat;		// color format
	DWORD			dwByteStride;		// width byte stride
	BOOL			bReverse;			// reverse screen
	BOOL			bShowFrontBuffer;	// Front(TRUE)/Back(FALSE) buffer's status (do not fix it.)
	BOOL			bFramePause;		// stop frame buffer change
	BOOL			bMovieSave;		// Saving to movie file...

	DisplayBuffer	Front;				// Front buffer
	DisplayBuffer	Back;				// Back buffer

#ifdef TESTDRIVE_DOCUMENT
	DisplayDecoder	decoder;				// virtual screen decoding override function pointer (Use TestDrive document implementation only.)
#endif
}DisplayConfig;
```

위의 각 변수들은 아래와 같은 아래와 같이 기술된다.

##### dwMemorySize
> 할당된 메모리의 byte 크기 (이 값은 변경하지 않는다.)

##### hWndHandle
> 디스플레이 도큐먼트의 윈도우 핸들. (64bit 컴파일된 바이너리는 지원하지 않는다.)

##### iWidth
> 너비 픽셀 크기

##### iHeight
> 높이 픽셀 크기

##### ColorFormat
> 디스플레이 픽셀 포멧, 아래의 포멧을 나타낸다.
	DISPLAY_FORMAT_ALPHA			: 8비트 alpha 포멧, A8
	DISPLAY_FORMAT_LUMINANCE		: 8비트 luminance 포멧, L8
	DISPLAY_FORMAT_LUMINANCE_ALPHA	: 16비트 luminance + alpha 포멧, L8A8
	DISPLAY_FORMAT_RGB_565			: 16비트 R(5) + G(6) + B(5) 포멧, R5G6B5
	DISPLAY_FORMAT_RGBA_4444		: 16비트 R(4) +  G(4) +  B(4) +  A(4) 포멧, R4G4B4A4
	DISPLAY_FORMAT_RGBA_5551		: 16비트 R(5) +  G(5) +  B(5) +  A(1) 포멧, R5G5B5A1
	DISPLAY_FORMAT_RGB_888			: 24비트 R(8) +  G(8) +  B(8) 포멧, R8G8B8
	DISPLAY_FORMAT_ABGR_8888		: 32비트 A(8) + B(8) +  G(8) +  R(8) 포멧, A8B8G8R8
	DISPLAY_FORMAT_RGBA_8888		: 32비트 R(8) + G(8) +  B(8) +  A(8) 포멧, R8G8B8A8
	DISPLAY_FORMAT_ARGB_8888		: 32비트 A(8) + R(8) +  G(8) +  B(8) 포멧, A8R8G8B8

##### dwByteStride
> 실제 너비의 총 바이트 수
	  이 값을 0으로 설정할 경우, (iWidth * ColorFormat 바이트 수) 로 자동 조절된다.

##### bReverse
> 화면의 위아래를 뒤집는다.

##### bShowFrontBuffer
> 현재 보이는 디스플레이가 전면 버퍼(True) 인지 후면(False) 인지 알린다. (이 값은 변경하지 않는다.)

##### bFramePause
> 속성 뷰에서 사용자가 설정한 "재생중 정지" 값을 알린다.
	  TestDrive 상의 속성 뷰에서 이 값을 True 변경하면 사용자 응용프로그램에서 이 값을 보고
	  잠시 대기할 수 있도록 Hint 를 주는 정보이다.
	  응용프로그램이 이 값을 관찰하지 않는다면 이 설정을 사용하여서는 안된다.

##### bMovieSave
> 현재 디스플레이 도큐먼트가 사용자 설정에 의해 동영상 저장 중임을 알린다.

##### Front
> 전면(Front) 버퍼의 메모리 Offset 주소와 갱신 여부를 알린다.
	  참조 메모리는 .profile 상 처음 생성한 메모리 모델에 해당한다.

##### Back
> 후면(Back) 버퍼의 메모리 Offset 주소와 갱신 여부를 알린다.
	  참조 메모리는 .profile 상 처음 생성한 메모리 모델에 해당한다.

##### decoder
> 디스플레이 도큐먼트의 출력이 위와 같은 일반적인 방식으로 출력이 불가능할 때 이 함수를
	  Override 하여, 출력 방법을 구현합니다.




### 속성 뷰
 
##### 화면 좌표계
>	: 화면의 X,Y 좌표계를 지정합니다.
 도큐먼트 출력창 아래에 다음처럼 출력됩니다.
 선택가능한 옵션은 아래와 같습니다.
 Screen	: 왼쪽 상위가 (0,0) 입니다.
 OpenGL	: 오른쪽 상위가 (0,0) 입니다.

##### 프론트 버퍼
>: 출력할 버퍼 속성을 지정합니다.
  True 일 경우 전면 버퍼를 False 일 경우 후면 버퍼가 실시간으로 출력됩니다.

##### 동영상 저장
> 동영상 저장을 활성화 합니다.

##### 동영상 FPS
> 동영상 저장할 FPS(frame per a second) 를 지정합니다.

##### 동영상 파일
> 동영상 저장할 파일을 지정합니다.
  저장될 위치는 %PROJECT%Program/ 에서 상대적인 위치입니다.

##### 재생중 정지
> 이 값을 True 로 설정하여 응용프로그램과 재생 중 잠시 정지할 수 있도록 합니다.
  이 기능은 전면 버퍼가 True 일때에만 사용 가능합니다.

##### 선호 스크린 모드
> 선호하는 스크린 모드를 지정합니다. 이 문자열은 "PREFERRED_SCREEN_MODE" 환경변수에 저장됩니다.
  응용프로그램은 환경 변수를 읽어 사용자가 원하는 스크린 모드를 선택 가능하게 하는 Hint 를 얻을 수 있습니다.

##### 선호 스크린 크기
> 선호하는 스크린 크기를 지정합니다. 이 문자열은 "PREFERRED_SCREEN_SIZE" 환경변수에 저장됩니다.
  응용프로그램은 환경 변수를 읽어 사용자가 원하는 스크린 크기를 결정 가능하게 하는 Hint 를 얻을 수 있습니다.

### 예제

Meitner 예제 프로젝트에 다음과 같이 수록되어 있습니다.

##### N-body simulation 예제
> 프로젝트 위치 : %PROJECT%Application/Example/Testbench/Nbody/.Project.bat
("프론트 버퍼" 설정을 True 설정하여야 깜빡임 현상이 나타나지 않습니다.)

##### Julia set fractal 예제
> 프로젝트 위치 : %PROJECT%Application/Example/Testbench/JuliaSet/.Project.bat
 ("프론트 버퍼" 설정을 True 설정하여야 깜빡임 현상이 나타나지 않습니다.)

### [:fa-arrow-left: Back](?top.md)
