---
layout: post
title: CodeEngn - Basic RCE - Level2
date: 2019-01-12 05:25
categories: CodeEngn
---
문제는 [**Basic RCE - Level2**](https://codeengn.com/challenges/basic/02)에서 
다운받으실 수 있습니다.
* * *
코드엔진 기초 첫번째 문제
**Basic RCE - Level2**
문제입니다.

시작합니다!

* * *

오늘의 문제는?

```
Korean : 
패스워드로 인증하는 실행파일이 손상되어 실행이 안되는 문제가 생겼다. 패스워드가 무엇인지 분석하시오 

English : 
The program that verifies the password got messed up and ceases to execute. Find out what the password is. 
```

프로그램이 손상되어 실행이 안된다...?

예?

진짜인가 실행했는데...
![nope](https://user-images.githubusercontent.com/46376448/51057078-e602f580-1627-11e9-89e5-338a65667245.png)
(ఠ ̥̆ ఠ) 
ㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋ 뭔문제야 ㅋㅋㅋㅋㅋㅋㅋ

그래서 PE View로 뜯어보았습니다.

![peview](https://user-images.githubusercontent.com/46376448/51057323-9e309e00-1628-11e9-91ef-dccbeaeda072.JPG)
뭐냐 이건...

파일 헤더들 어디감??

그래서 내려봤는데...

수상한 부분 2군데가있군요

첫번째는
![message](https://user-images.githubusercontent.com/46376448/51057430-ed76ce80-1628-11e9-8dff-0c90452db5a6.JPG)

인데... 뭔가 메세지 같아서 뽑아봤습니다. 유니코드로 작성된거같네요.

```
ArturDents CrackMe #1 MS Sans Serif 

Check It! About Rules just find the unlock code no patching allowed
It's very easy so dont exspect to much Greeting to the whole TNP
especially Folko for helping me to make my first steps in Win32ASM
```
뭐... 해석은 모르겠고... 패스워드는 없는듯하네요.
다음~ (´◒`๑)

![password_blind](https://user-images.githubusercontent.com/46376448/51057594-6d9d3400-1629-11e9-9181-383e6db71b94.png)
이부분인데 마지막 부분이 패스워드인듯 합니다. 

* * *

뭔가 이번문제가... 조금 애매한듯합니다 ㅋㅋㅋㅋㅋ 야매로 푼거같기도하고...
다음에는 Basic RCE - Level3로 뵙시다! 그럼 이만!
