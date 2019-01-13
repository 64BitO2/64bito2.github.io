---
layout: post
title: CodeEngn - Basic RCE - Level5
date: 2019-01-14 04:09
categories: CodeEngn
---
문제는 [**Basic RCE - Level5**](https://codeengn.com/challenges/basic/05)에서 
다운받으실 수 있습니다.
* * *
코드엔진 기초 첫번째 문제
**Basic RCE - Level5**
문제입니다.

시작합니다!

* * *

문제!

```
Korean : 
이 프로그램의 등록키는 무엇인가 

English : 
The registration key of this program is? 
```

심플하네요. 

동적 분석부터 시작합시다!

![main](https://user-images.githubusercontent.com/46376448/51089651-3c9d3a80-17b4-11e9-8e51-b007521958a7.JPG)
실행하면 다음과 같은 화면이 뜨고요.

Register now!를 누르니
![fail](https://user-images.githubusercontent.com/46376448/51089649-3c9d3a80-17b4-11e9-8504-c1c5f628161f.JPG)
라네요!

근데 PEID를 올려봤는데..

![peid](https://user-images.githubusercontent.com/46376448/51089644-3b6c0d80-17b4-11e9-81c0-2241cfeefb43.JPG)

UPX로 압축되어있군요. 풀어줍시다

![unpacked](https://user-images.githubusercontent.com/46376448/51089647-3c04a400-17b4-11e9-9304-35b310dae71c.JPG)

풀어주고 나서, 올리디버거로 엽시다!

![olly](https://user-images.githubusercontent.com/46376448/51089643-3b6c0d80-17b4-11e9-9758-4651398ab925.JPG)

아까 찾은
![fail](https://user-images.githubusercontent.com/46376448/51089649-3c9d3a80-17b4-11e9-8504-c1c5f628161f.JPG)
에서 힌트를 얻어 스트링찾으러 갑시다!

갔는데
![strings](https://user-images.githubusercontent.com/46376448/51089646-3c04a400-17b4-11e9-8e3a-ae6b236daadc.JPG)
2개가 있네요?

이럴때는 
![bp_string](https://user-images.githubusercontent.com/46376448/51089648-3c9d3a80-17b4-11e9-8ae2-75a82dd01fce.JPG)
2개다 BP를 걸고 갑시다.

다시 
![main](https://user-images.githubusercontent.com/46376448/51089651-3c9d3a80-17b4-11e9-8e51-b007521958a7.JPG)
Register now!를 누르면

![if](https://user-images.githubusercontent.com/46376448/51089824-e67dc680-17b6-11e9-9bd6-ab900c0eb2e8.JPG)
이곳으로 오네요

위쪽에 보니 2개가 보이네요
![name](https://user-images.githubusercontent.com/46376448/51089838-03b29500-17b7-11e9-9e79-4d1e9ba8ea6c.JPG)
우리가 입력한 이름을 Registered User과 비교를 하고있구요.

![serial](https://user-images.githubusercontent.com/46376448/51089645-3c04a400-17b4-11e9-9f60-ce94a1fe4e42.png)
우리가 입력한 시리얼과 정답인거같은 시리얼을 비교하고있습니다.
이게 답인것같네요!