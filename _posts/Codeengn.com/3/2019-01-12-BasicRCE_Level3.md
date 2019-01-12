---
layout: post
title: CodeEngn - Basic RCE - Level3
date: 2019-01-12 21:39
categories: CodeEngn
---
문제는 [**Basic RCE - Level3**](https://codeengn.com/challenges/basic/03)에서 
다운받으실 수 있습니다.
* * *
코드엔진 기초 첫번째 문제
**Basic RCE - Level3**
문제입니다.

시작합니다!

* * *

오늘의 문제는?

```
Korean : 
비주얼베이직에서 스트링 비교함수 이름은? 

English : 
What is the name of the Visual Basic function that compares two strings? 
```
이네욥.

일단 동적 분석부터 시작합니다.

![first](https://user-images.githubusercontent.com/46376448/51058647-ca4e1e00-162c-11e9-8cad-233d33ee5d79.JPG)
뭔 언어야;
뭐야;;  (｡ŏ﹏ŏ)｡ 

모르겠다.. 다음...

![main](https://user-images.githubusercontent.com/46376448/51058648-ca4e1e00-162c-11e9-9021-8fd64e3bd0dc.JPG)
가운데 코드 입력하면되는것 같네요.

아무거나 입력했더니
![fail](https://user-images.githubusercontent.com/46376448/51058646-ca4e1e00-162c-11e9-8b67-f6c898871b32.JPG)
안된데여... 그런거같아요...

뭐 그렇다 치고.. 이제 정적 분석으로 들어갑시다.

![olly](https://user-images.githubusercontent.com/46376448/51061979-806b3500-1638-11e9-81f6-89c26cc88ce9.JPG)
비주얼 베이직으로 짜여진 프로그램 같습니다.

우리가 얻은 힌트는 
![fail](https://user-images.githubusercontent.com/46376448/51058646-ca4e1e00-162c-11e9-8b67-f6c898871b32.JPG)
의 문자열이죠.

오른쪽 클릭 -> Search for -> All referenced strings
로 찾아보니

![string](https://user-images.githubusercontent.com/46376448/51062091-e35ccc00-1638-11e9-8825-b94c6e619045.JPG)
보이네요!

더블클릭하면,

![if jpg](https://user-images.githubusercontent.com/46376448/51072374-150a7d00-16a3-11e9-95e1-c84027fb0357.png)
이 부분으로 넘어오는데.

```
CPU Disasm
Address   Hex dump          Command                                  Comments
00402A2F  |.  E8 16E7FFFF   CALL <JMP.&MSVBVM50.########>         ; \MSVBVM50.#######, String Compare
```

누가봐도 이게 문자열 비교함수인듯합니다 ㅋㅋㅋ

정답 제출합시다!
