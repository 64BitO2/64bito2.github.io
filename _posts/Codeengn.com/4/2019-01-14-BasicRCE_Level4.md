---
layout: post
title: CodeEngn - Basic RCE - Level4
date: 2019-01-14 03:03
categories: CodeEngn
---
문제는 [**Basic RCE - Level4**](https://codeengn.com/challenges/basic/04)에서 
다운받으실 수 있습니다.
* * *
코드엔진 기초 첫번째 문제
**Basic RCE - Level4**
문제입니다.

시작합니다!

* * *

오늘은!

```
Korean : 
이 프로그램은 디버거 프로그램을 탐지하는 기능을 갖고 있다. 디버거를 탐지하는 함수의 이름은 무엇인가 

English : 
This program can detect debuggers. Find out the name of the debugger detecting function the program uses. 
```
이군요.

먼저 동적분석부터 시작합시다.

* * *

프로그램을 실행시켜보니

![normal](https://user-images.githubusercontent.com/46376448/51088928-1625d200-17a9-11e9-89f7-59a1c780fcd0.JPG)
정상이라는 말이 반복되서 나오고 있네요.

디버거 프로그램을 탐지한다고 했으니, 올리디버거를 연결해보았더니...

![debugged](https://user-images.githubusercontent.com/46376448/51088925-158d3b80-17a9-11e9-95e6-a0a8784aa3a9.JPG)
이번엔 디버깅 당함이라는 말을 반복하네요.

사실 이번문제는 조금 편하다고 생각하는게, 다른걸 찾는것이 아닌...
"함수"만 찾으면 되니까! 라는 점이 조금 편하네요.

왜 그러냐고요? 계속 한번 봅시다.

일단 올리 디버거로 켜봅시다.

![olly](https://user-images.githubusercontent.com/46376448/51088929-1625d200-17a9-11e9-8c48-285f3b247698.JPG)
자 그리고, 마우스 오른쪽 클릭후,

Search For -> All intermodular calls를 선택하면

![inter](https://user-images.githubusercontent.com/46376448/51088997-cb588a00-17a9-11e9-83f5-2d1c2c030f67.JPG)
다음과같이 호출되는 함수들만 모아볼 수 있습니다.

여기서 그 함수가 있겠죠.

내리다 보니...

이름이 수상한 함수 하나가 있군요.
![function](https://user-images.githubusercontent.com/46376448/51088927-158d3b80-17a9-11e9-8c9c-0e75f399dc0f.png)

이 함수가 정답인듯 합니다.

* * *

이번 문제는 올리디버거의 기능으로 간단하게 해결할 수 있었던것같습니다.

그럼 다음문제에서 뵙겠습니다!