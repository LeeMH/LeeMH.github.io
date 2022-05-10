---
title: InteliJ 프로토콜 버퍼 파일이 import 안되는 경우
category: TIL
order: 9997
---

## 1. 개요

* 갑자기 프로토콜 버퍼 파일이 import 가 안된다.
* 오늘 kotlin 버전 업데이트하면서 InteliJ 업데이트한게 문제인가?
* 버전을 다운그레이드하고 재설치/재시작을 반복하며 삽질을 반복했다.
* 삽질완료후 몇달전에 했던 삽질과 비슷한것을 알게되었고, 미래에 동일한 일이 없기를 바라며 끄적여 본다.

## 2. 에러현상

* 아래 그림과 같이 프로토콜 버퍼 파일이 import 가 되지 않는다.
![screenshot](../../../images/202205/intelij_import_error.png)


## 3. 확인절차

* protocol buffer 플러그인 설치가 되었는지 확인한다. (플러그인이 직접적인 원인은 아니겠지만, 해당 플러그인을 설치하라는 설명이 자주 나오는것으로 봐서 일단 설치!!)
![screenshot](../../../images/202205/intelij_plugin_install.png)

* source generate 작업을 수행해보자. 이 작업을 돌리면 source generate 작업이 수행되고 없어서 발생한 문제라면 해결이 된다.
![screenshot](../../../images/202205/intelij_maven_source_generate.png)

* source generate 결과 디렉토리가 source root로 설정되어 있는지 확인. java/main/src 와 동일한 색상이라면 OK
![screenshot](../../../images/202205/intelij_source_root.png)

* 여기까지 해도 해결이 되지 않았다면, import 문제가 발생한 파일을 열어본다. 분명 파일은 존재하지만 import 에러가 발생했을 것이다.
이경우, 파일 상단의 메세지를 보면 파일 사이즈는 4.13메가 이지만 설정에는 2.56메가 limit으로 설정되어 있다고 나온다. 
이것이 원인이다.
![screenshot](../../../images/202205/intelij_protobuf_file.png)

* InteliJ 설치 디렉토리에서 idea.property 파일을 열고, 아래 limit 부분 설정을 변경한다. 라인, 사이즈 모두 0 하나 추가하면 여유있지 않을까 싶다.
![screenshot](../../../images/202205/intelij_property.png)
