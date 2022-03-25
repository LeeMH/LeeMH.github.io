---
title: java process 분석 명령어
category: java
order: 9999
---

## 개요

요즈음 대부분 서버 환경이 컨테이너화 되어 있지만,
그래도 가끔 서버에(혹은 로컬에서) 접속하여 java process를 분석해야 하는 경우가 있다.

장애 또는 이상현상 발생시 구체적인 상황을 파악하기 위한 명령어, 사용법을 정리 한다.

## 1. java process 찾는 방법

jdk에 내장된 jps 명령어를 사용하여 java 프로세스를 찾는다.

```bash
$ jps
```

아래와 같이 출력되며, 20780, 9390 두개가 java process 의 PID 

![screenshot](../../../images/202203/java-cmd-jps.png)


## 2. process 상세 출력

ps 명령어를 사용하여 정보를 출력한다.

```bash
$ ps -ef | grep <PID>
```

아래와 같이 출력되며, 프로세스 구동시 사용된 jvm 옵션이 출력된다.

![screenshot](../../../images/202203/java-cmd-ps.png)

## 3. jvm 메모리 상황 및 gc 정보 출력 (백분율)

jstat -gcutil 명령어를 사용하여 정보를 출력한다.
주의할점은 출력되는 수치는 백분율 값이다. 

```bash
$ jstat -gcutil <PID> <interval>
```

아래와 같이 출력된다.

* S0, S1 : 서바이벌 영역
* E : 에덴 영역
* O : old 영역
* FGC : Full GC 횟수
* FGCT : Full GC 시간
* GCT : GC 총 시간

![screenshot](../../../images/202203/java-cmd-jstat1.png)


## 4. jvm 메모리 상황 및 gc 정보 출력 (실제값)

jstat -gc 명령어를 사용하여 정보를 출력한다.

```bash
$ jstat -gc <PID> <interval>
```

아래와 같이 출력된다.
위 명령어와 다른점은 백분율이 아닌 실제 값으로 표시되는 점이다.
따라서 전체(C), 사용량(U) 두가지 항목으로 표시된다.

* OC : old 총 사이즈
* OU : old 사용 사이즈

![screenshot](../../../images/202203/java-cmd-jstat2.png)


## 5. heap memory histogram

jmap 명령어를 사용하여 정보를 출력한다.

```bash
$ jmap -histo <PID> | more
```

아래와 같이, 인스턴스 갯수와 메모리 사이즈가 출력된다.
![screenshot](../../../images/202203/java-cmd-jmap.png)

live 객체만 출력하고자 하면, 아래와 같이 :live 옵션을 추가한다.
단, 이경우 GC가 발생하므로 실환경에서는 주의가 필요하다.

```bash
$ jmap -histo:live <PID> | more
```

## 6. heap dump

jmap 명령어를 사용하여 정보를 출력한다.
heap dump 작업을 진행하는동안 프로세스가 멈춘다. 
실환경에서 사용시 주의가 필요하다.

```bash
$ jmap -dump:format=b,file=<파일명> <PID>
```

덤프된 파일은 MAT 등 힙메모리 분석툴을 사용해서 분석이 가능하다.
아래는 MAT 로 heap dump 파일을 load 한 케이스이다.

![screenshot](../../../images/202203/java-cmd-mat.png)

## 7. thread dump

jstack 명령어를 사용하여 정보를 출력한다.

```bash
$ jstack <PID>
```

![screenshot](../../../images/202203/java-cmd-jstack.png)

## 8. java process thread 단위 cpu, memory 점유율 확인

jstat 명령어를 사용하여 정보를 출력한다.

```bash
$ ps -mo lwp,pcpu,pmem <PID>
```

![screenshot](../../../images/202203/java-cmd-ps-thread.png)

* lwp : 쓰레드 ID
* pcpu : cpu 사용량
* pmem : memory 사용량


## 9. cpu 많이 점유하는 thread 찾기

8번의 20781 쓰레드가 cpu 를 과하게 사용한다고 가정하자.
해당 쓰레드가 무엇인지 알아야 한다.

우선 20781 를 hex 값으로 변경하자. (계산기 -> 프로그래머용을 사용하면 쉽게 변경할수 있다.)
20781 의 hex 값은 512d 이다.

![screenshot](../../../images/202203/java-cmd-ps-thread-calc.png)

이제 쓰레드 덤프를 확인하자.
find 명령을 사용하여 0x512d 를 찾아보자.
아래의 경우에서는 destroyJavaVM 이란 쓰레드 이다.
당연히 실제 업무에서 장애 또는 이상현상 분석시에는 "업무" 관련된 쓰레드가 잡힐것이고,
스택 트레이스가 찍혀 있기때문에 어느 부분에서 문제가 있는지 추적이 가능하다.

![screenshot](../../../images/202203/java-cmd-ps-thread-search.png)

