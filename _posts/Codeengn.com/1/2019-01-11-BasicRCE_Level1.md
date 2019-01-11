---
layout: post
title: CodeEngn - Basic RCE - Level1
date: 2019-01-11 20:21
categories: CodeEngn.com
---
문제는 [**Basic RCE - Level1**](https://codeengn.com/challenges/basic/01)에서 
다운받으실 수 있습니다.
* * *
코드엔진 기초 첫번째 문제
**Basic RCE - Level1**
문제입니다.

시작합니다!

* * *

자 오늘의 문제입니다!
```
Korean : 
HDD를 CD-Rom으로 인식시키기 위해서는 GetDriveTypeA의 리턴값이 무엇이 되어야 하는가 

English : 
What value must GetDriveTypeA return in order to make the computer recognize the HDD as a CD-Rom
```
사실 저는 문제 파일 다운받기 전에도 무슨 프로그램인지 알겠더라고요ㅋㅋㅋ

**GetDriveTypeA**가 쓰이는 유명한 문제가 있거든요ㅋㅋㅋㅋㅋ

그래도 일단 풀어봅시다!

_ _ _


먼저 동적 분석부터 합니다.

```
Q.동적 분석이란?

A.프로그램을 직접 실행하면서 프로그램의 흐름, 리버싱할때 힌트가 될만한 것들을 찾는겁니다.
```

프로그램을 실행해보면

![main](https://user-images.githubusercontent.com/46376448/51032005-d3fe6400-15e1-11e9-8d6a-364f4ae6c22b.JPG)
이런 창이뜨고요.

버튼을 누르면
![fail](https://user-images.githubusercontent.com/46376448/51032004-d3fe6400-15e1-11e9-87bd-fbed4f7f7f3c.JPG)
다음창이 뜹니다.

여기서 이 프로그램이 원하는건 
```
Make me think your HD is a CD-Rom.
(당신의 HD(?)를 CD-Rom으로 생각하게 해줘.)
```
아마 HD는 HDD(하드디스크)를 이야기하는것 같네요.

그럼 이제 직접 뜯어봅시다!

* * *

![olly](https://user-images.githubusercontent.com/46376448/51032141-48390780-15e2-11e9-88e7-a43383dd3824.JPG)
올리 디버거에 프로그램을 넣어보니
다음과 같이 나오네요!!

코드가 굉장히 깔끔한걸로 보아서는 아마 어셈블리어로 직접 제작된 프로그램같습니다.

한번 들여다 봅시다.

![main_as](https://user-images.githubusercontent.com/46376448/51032445-5e939300-15e3-11e9-95b2-0e6ce3320d46.JPG)
먼저 WinAPI인 **MessageBoxA**를 통해 
![main](https://user-images.githubusercontent.com/46376448/51032005-d3fe6400-15e1-11e9-8d6a-364f4ae6c22b.JPG)
창을 뛰우고,


![getdrice](https://user-images.githubusercontent.com/46376448/51032443-5e939300-15e3-11e9-8f96-a51f112c73f4.JPG)

**GetDriveTypeA**에 인자를 **"C:\"** 주고 그 값을 계산한 후,  

![if](https://user-images.githubusercontent.com/46376448/51032444-5e939300-15e3-11e9-93c4-618a5fad100f.JPG)

그 결과를 가지고 **JE SHORT 0040103D**를 통해 분기시키는 모습을 확인할 수 있습니다.

이 결과를 가지고 플로우차트를 만들면

![flow](https://user-images.githubusercontent.com/46376448/51032531-bf22d000-15e3-11e9-929b-98bd0c55e19d.JPG)

다음과 같겠네요. 

그럼우리가 분석해야할 곳은 루틴이겠네요!

* * *

![getdrice](https://user-images.githubusercontent.com/46376448/51032443-5e939300-15e3-11e9-8f96-a51f112c73f4.JPG)

자! 우리가 유심히 보아야할 곳입니다.

자세히 보면

```assembly
CPU Disasm
Address   Hex dump          Command                                  Comments
00401013  |.  68 94204000   PUSH OFFSET 00402094                     ; /RootPath = "c:\"
00401018  |.  E8 38000000   CALL <JMP.&KERNEL32.GetDriveTypeA>       ; \KERNEL32.GetDriveTypeA

```
를 통해서 GetDriveTypeA라는 함수를 호출하고있습니다.

구글링을 통해 찾아봅시다.

```C++
UINT GetDriveTypeA(
  LPCSTR lpRootPathName
);
```

![rv](https://user-images.githubusercontent.com/46376448/51032695-50924200-15e4-11e9-89fb-5e26d002caf4.JPG)

이정도 정보면 충분하겠네요.

**"C:\"**는 HDD이니까
**GetDriveTypeA**는 HDD는 설명에 있듯이 **DRIVE_FIXED** | **3** 를 반환하겠네요.

그럼 CD-Rom으로 인식 시켜달라니까 5으로 리턴되야하겠네요.

![eax_r](https://user-images.githubusercontent.com/46376448/51033477-d31c0100-15e6-11e9-921c-3420a3e409e5.JPG)
EAX를 통해서 3을 리턴받은 모습입니다.

그리고나서
```
CPU Disasm
Address   Hex dump          Command       
0040101D  |.  46            INC ESI
0040101E  |.  48            DEC EAX
0040101F  |.  EB 00         JMP SHORT 00401021
00401021  |>  46            INC ESI
00401022  |.  46            INC ESI
00401023  |.  48            DEC EAX
00401024  |.  3BC6          CMP EAX,ESI
00401026  |.  74 15         JE SHORT 0040103D
```
약간의 계산 과정후,
```
CPU Disasm
Address   Hex dump          Command                
00401024  |.  3BC6          CMP EAX,ESI
00401026  |.  74 15         JE SHORT 0040103D
```
EAX와 ESI를 비교를 하고, 분기를 시킵니다.

```
CPU Disasm
Address   Hex dump          Command                                  Comments
00401028  |.  6A 00         PUSH 0                                ; /Type = MB_OK|MB_DEFBUTTON1|MB_APPLMODAL
0040102A  |.  68 35204000   PUSH OFFSET 00402035                  ; |Caption = "Error"
0040102F  |.  68 3B204000   PUSH OFFSET 0040203B                  ; |Text = "Nah... This is not a CD-ROM Drive!"
00401034  |.  6A 00         PUSH 0                                ; |hOwner = NULL
00401036  |.  E8 26000000   CALL <JMP.&USER32.MessageBoxA>        ; \USER32.MessageBoxA
0040103B  |.  EB 13         JMP SHORT 00401050
0040103D  |>  6A 00         PUSH 0                                ; /Type = MB_OK|MB_DEFBUTTON1|MB_APPLMODAL
0040103F  |.  68 5E204000   PUSH OFFSET 0040205E                  ; |Caption = "YEAH!"
00401044  |.  68 64204000   PUSH OFFSET 00402064      ; |Text = "Ok, I really think that your HD is a CD-ROM! :p"
00401049  |.  6A 00         PUSH 0                                ; |hOwner = NULL
0040104B  |.  E8 11000000   CALL <JMP.&USER32.MessageBoxA>        ; \USER32.MessageBoxA
00401050  \>  E8 06000000   CALL <JMP.&KERNEL32.ExitProcess>      ; \KERNEL32.ExitProcess
```
그리고 성공과 실패를 구분하네요.


C언어로 간단히 나타내자면

```C
EAX = GetDriveTypeA("c:\\");

EAX -= 2;
ESI += 3;

if(EAX == ESI)
	성공!
else
	실패!
```
이니까 

ESI는 -1에서  +3이니까 2가 고정일것이고...

HDD이면 EAX가 3 -> 1이 될것이고....
CD-Rom이면 EAX가 5 -> 3....?

CD-Rom이여도 안되는데요ㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋ
정답이 될려면 네트워크 드라이브여야겠네요 ㅋㅋㅋㅋ
* * *

그러면 CD-Rom으로 인자를 바꿔주어도 안되는걸 알았으니, 그냥 분기점인
```
00401026  | 74 15    JE SHORT 0040103D
```
인 JE를

무조건 점프인 JMP로 바꿔주면 되겠네요.
```
000401026  | EB 15    JMP SHORT 0040103D
```

그리고 실행해보면!!

![successs](https://user-images.githubusercontent.com/46376448/51033844-1034c300-15e8-11e9-9391-488de3b807cb.JPG)

성공창이 떴습니다!

* * *

자 이제 마지막으로 패치파일을 저장해줍시다!

고칠 코드자리인 **00401026**을 오른쪽 클릭,
Edit -> Copy to executable
그러면 File창이 뜨는데요.

거기에서 다시 오른쪽클릭!
Save File을 해주시면됩니다!

![savefile](https://user-images.githubusercontent.com/46376448/51033983-79b4d180-15e8-11e9-8f6f-41b4aeadee1a.JPG)

저장 후, 패치 파일을 실행시켜보면?

![perfect](https://user-images.githubusercontent.com/46376448/51034032-a7017f80-15e8-11e9-851c-ed3eee895a99.JPG)
성공!



