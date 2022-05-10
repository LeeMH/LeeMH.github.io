---
title: docker 할당 ip 변경하기
category: TIL
order: 9999
---

## 1. 개요

* dev, qa, rc 까지 거쳐서 테스트된 서비스가 리얼에서 접속문제가 발생했다.
* 애플리케이션 변경이력과 빌드 이력을 살펴보았으나, 변경은 없었다.
* 무엇이 문제일까 찾던중 docker 에서 사용하는 172 대역의 ip와 LAN 의 ip 대역과 충돌하면서 발생하는 문제라는것을 알게 되었다.

## 2. docker 할당 ip range 변경

* docker 설정화일에 bip 라는 항목을 추가하자.
* 파일위치는 /etc/docker/daemon.json 이다.
* 만약 파일이 존재한다면, 아래 내용을 추가하고 없다면 daemon.json 파일을 추가하고 내용을 추가한다.

```bash
{
  "bip": "192.168.0.1/16"
}
```

## 3. 확인

* docker 서비스를 재기동 한다.
* 그리고 ifconfig로 docker ip를 확인한다.
* 아래와 같이 docker0의 ip가 192.168로 시작하면 정상적으로 변경된 것이다.

![screenshot](../../../images/202205/docker_ip.png)