# GPASM 사용 설명서


기본적인 문법 설명입니다.

```
#include filename_string	 : C의 #include와 같은 기능을 한다.
#function    function_name	description_function
	function_name		    : string 타입으로 ""로 문자열을 표시해야 한다.
	description_function	 : 긴 함수정의 문법들로 나열되어 동작을 수행하도록 한다.
		@M message_string
		#M [message_string]
									  : 로그에 메시지를 출력한다.
		(@,#)E						: 강제로 에러를 발생시킨다.
		(@,#)N						: 다음 저장될 명령어로 이동한다.
		(@,#)P						: 이전의 저장된 명령어로 이동한다.
		(@,#)L						: line feed, 코드 라인이 없으면 한번 더 받는다.
		(@,#)I variable_(name,int,hex,float,binary,string)	
									  : Insert line, 코드 라인에 추가시킨다.
		@F name_function
		#F [name_function]				: 다른 함수 동작을 함수명으로 호출한다.
		@V variable_name
		#V [variable_name]				: 현재 참조변수를 설정한다.
									  variable_name으로 PC를 설정하면 현재 PC address가 현재 참조 변수로 설정된다.
		@$(i,f,s,n) variable_name
		#$(i,f,s,n) [variable_name]			: 새로운 변수이름을 추가시키고 속성을 (i:int, f:float, s:string, n:name)로 정하며, 그 변수를 참조변수로 지정한다.
		(@,#)A						: 새로운 PC address tag 가 추가되고 이것은 현재 PC address로 설정한다.
									  이것은 "@V PC" 문법을 통하여 현재 참조변수로 설정할 수 있다.
		@T (tag_address,{{,{,},#,$,%)
		#T [tag_address]				: 어드레스 테그를 추가시키거나 현재 함수 깊이를 참조변수에 설정한다.
									  '{{' 는 현재 함수 깊이를 증가 시키고 현재 함수가 루프임을 나타낸다.
									  '{' 는 현재 함수 깊이를 증가 시킨다.
									  '}' 는 현재 함수 깊이를 감소 시킨다. (감소한 함수 깊이가 0일 때, 함수 번호가 1 증가한다.)
									  '#' 는 현재 참조변수에 함수 깊이를 넣는다. (함수 깊이는 최초 0을 가리키며, "@T{"또는 "@T{{" 명령 이후 일때 1 증가되고 "@T}" 명령 이후 1 감소한다.)
									  '$' 는 현재 참조변수에 함수 번호를 넣는다. (함수 번호는 최초 0을 가리키며, 함수 깊이가 1일 때 "@T}" 명령 이후 함수 깊이는 1 감소해 0이 되고 함수 번호는 1 증가된다.)
									  '%' 는 현재 참조변수에 "@T{{" 에 의한 루프문 내에 있으면 1을 그렇지 않으면 0을 넣는다.
									  '{{' 또는 '{'과 '}'는 서로 페어로 사용되어야 한다.
									  만약 '}'이 '{'또는 '{{'보다 먼저 선언되어 있거나, 서로 쌍의 개수로 맞지 않을 경우 에러로 처리된다.
		
		@(=$,=,+,-,*,/,<,>,&,|,^,~) variable_(name,int,hex,float,binary)
		#(=$,=,+,-,*,/,<,>,&,|,^,~) [variable_(name,int,hex,float,binary)]
									: 참조변수의 값을 지정한 변수나 상수을 (=$(대입,타입일치),=(대입),+(덧셈),-(뺄셈),*(곱셈),/(나눗셈),<(왼쪽 쉬프트),>(왼쪽 쉬프트)
									  ,&(AND),|(OR),^(XOR),~(NOT)) 연산을 한다.
									  문자열 변수나 이름 변수일 경우 '=' 연산자만 사용가능하다.
									  단, 연산 대상 값이 초기화 되어 있지 않으면 오류로 처리한다.
		@? variable_(name,int,hex,float,binary,'#','%') true_function {: false_function}
		#? [variable_(name,int,hex,float,binary)] true_function {: false_function}
									: 참조변수가 variable과 같으면 true 함수를 수행하고 아니면 false 함수로 수행 후 자신의 다음 함수를 수행한다.
									  function이 들어갈 자리에 '*' 특수 문자가 있을 경우 이후의 함수 정의 문법들을 무시하고,
									  # 문자일 경우 다음 함수 정의 문법이 바로 실행되도록 하며,
									  @ 문자일 경우 다시 function 이름을 받아 조건에 맞게 실행 후 이후의 함수 정의 문법들을 무시한다.
									  "@ ?variable_" 에서 '#'일 경우 참조변수가 정수일 때 true, 그렇지 않으면 false를 지시한다.
									  "@ ?variable_" 에서 '%'일 경우 참조변수가 실수일 때 true, 그렇지 않으면 false를 지시한다.
		(@,#)!{!} token_string true_function {: false_function}
									: token 문자열이 있으면 true 함수를 수행하고 아니면 false 함수로 수행 후 자신의 다음 함수를 수행한다.
									  function이 들어갈 자리에 '*' 특수 문자가 있을 경우 이후의 함수 정의 문법들을 무시하고,
									  # 문자일 경우 다음 함수 정의 문법이 바로 실행되도록 하며,
									  @ 문자일 경우 다시 function 이름을 받아 조건에 맞게 실행 후 이후의 함수 정의 문법들을 무시한다.
									  !가 두번 사용되면 토큰 스트링을 비교 후 discard 되지 않는다.
		(@,#)C token_string true_function {: false_function}
									: 현재 문자열이나 이름 참조변수에 token 문장 존재하면 true 함수를 수행하고 아니면 false 함수로 수행 후 자신의 다음 함수를 수행한다.
									true/false 조건 판별 이외에는 "(@,#)!" 과 동작이 동일하다.
									만드시 먼저 문자열이나 이름 참조변수가 먼저 선택되어야 한다.
		(@,#)W width_int true_function {: false_function}
									: 현재 문자열 길이가 width와 같으면 true 함수를 수행하고 아니면 false 함수로 수행 후 자신의 다음 함수를 수행한다.
									true/false 조건 판별 이외에는 "(@,#)!" 과 동작이 동일하다.
									만드시 먼저 문자열이나 이름 참조변수가 먼저 선택되어야 한다.
		@R{((,),[,],{,},@)} bitwidth_int position_int dest_address
		#R{@} bitwidth_int position_int [dest_address]
									: 현재 명령어의 position 번째 비트부터 bitwidth 만큼 dest와의 상대 주소 값을 저장한다.
		@D{((,),[,],{,},@)} bitwidth_int position_int dest_address
		#D{@} bitwidth_int position_int [dest_address]
									: 현재 명령어의 position 번째 비트부터 bitwidth 만큼 dest의 절대 주소 값을 저장한다.
									  @R과 @D에서 '(', ')'은 현재 루프의 시작과 끝 주소를 말하며,
									  '[', ']'은 현재 함수의 깊이 레벨0의 시작과 끝을 말한다.
									  '{', '}'는 현재 함수 깊이의 시작과 끝을 말한다.
									  dest_addres는 동일한 함수레벨에서 선언된 주소를 가리키며, @ 선언이 있으면 함수깊이 0번에 선언한 주소를 가리킨다.
									  "()[]{}"를 사용할 경우 dest_address는 사용되지 않는다.
		@S bitwidth_int position_int dest_(name,int,hex,binary)
		#S bitwidth_int position_int [dest_(name,int,hex,binary)]
									: 현재 명령어의 position 번째 비트부터 bitwidth 만큼 dest의 변수 값이나 상수 값을 저장한다.
		@@ custom_function_int			: 특수 설계된 함수 custom_function_int번을 실행한다.
```

> ************************** 참조 **************************
function 동작 문법에서
*_function	: 함수의 동작을 지정하는 문자열로써 그 라인의 끝까지 동작 문법들로 정의된다.
*_string	: ""로 묶여있는 문자열을 의미한다. ex) "home"
*_name	: 연속된 문자, 숫자, '_' 로 이루어진 이름을 의미한다. 단 첫 시작은 문자로 시작되어야 한다. ex) Tech_12
*_int		: 정수를 의미한다. ex) 1, 4, 6346, -1534, 0
*_hex	: 16진수를 의미한다. ex) 0x59, 0x4F00
*_float	: 실수를 의미한다. ex) -0.054, 11254.
*_binary	: 이진수를 의미한다. ex) 0b00101, 0b10111111101
()		: () 안에 상위의 속성을 ',' 문자로 구분하고 나열하여 그 중 하나만 선언 가능함을 의미한다.
{}		: {} 안의 문법에 맞는 문장이 존재할 경우만 받아 들인다.
[]		: #function    함수정의 문법에서가 아닌 소스에서 얻는다.

