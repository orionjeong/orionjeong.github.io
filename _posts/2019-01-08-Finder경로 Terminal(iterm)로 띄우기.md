---
layout: post
title:  Finder경로 Terminal(iterm)로 띄우기
date:   2019-01-08
description: Finder에서 바로 해당 결로 Terminal로 띄우기
tags: [tip]
category: tips
---


## Finder 해당경로를 Terminal로 띄우기



블로그들을 찾아보니 여러가지 방식들이 존재한다. 

1. Drag & drop - 미리 터미널을 열어야한다. 당연히 비효율적이다. 
2. 키보드 단축키 서비스등록 - 왜인지 작동을 안한다.
3. Go2Shell 응용프로그램 이용 - iterm현재버전에서 작동을 안한다. 

차라리 AppleScript Editor를 통해서 응용프로그램을 만들자. 



**응용프로그램 만드는 방법**

1. AppleScript Editor를 켠다

2. script를 입력한다.(finder경로를 Iterm으로 여는 스크립트)

   ![image-20190107234546844](/assets/img/image-20190107234546844.png)

3. 이름을 정하고 파일 포맷을 응용프로그램으로 만든 후 저장한다.

   ![image-20190107234910534](/assets/img/image-20190107234910534.png)



1. 응용프로그램을 저장한 폴더에서 command + 드래그 하여 도구에 추가한다.(상단에 드래그하면 알아서 추가됨)

   ![image-20190107235030133](/assets/img/image-20190107235030133.png)

2. 도구에 추가한 아이콘을 누르면 finder 현재 폴더 경로가 iterm에서 켜진다. 