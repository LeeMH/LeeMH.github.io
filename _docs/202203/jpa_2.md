---
title: jpa 다대일 연관관계
category: java
order: 9997
---


## 1. 연관관계의 주인

* 단방향 연관관계를 우선 시도
* 업무적으로 양방향 관계가 필요한 경우 반드시 관계의 주인 설정 필요
* FK 가 존재하는 테이블(클래스)가 연관관계의 주인이 되어야 한다
  * 주인쪽은 JoinColumn 애노테이션을 사용
  * 참조하는 쪽은 mappedBy 애노테이션을 사용

## 2. @ManyToOne

* 다대일(N:1) 연관관계에서 N쪽을 설정
  * 예시) 직원(N) <--> 팀(1)
* @JoinColumn 애노테이션을 사용해서 join에 사용할 FK 필드명을 지정

## 2. @OneToMany

* 다대일(N:1) 연관관계에서 1쪽을 설정
* @mappedBy 애노테이션을 사용해서 N쪽에서 참조하는 필드명을 지정

* schema 속성을 이용하여, 테이터베이스 schema를 지정가능

## 3. @Id

* pk 속성의 필드에 매핑

## 4. @Column

* 컬럼필드에 매핑
* 주요속성
  * name : 테이블 컬럼명을 지정
  * nullable : 테이블 not null 속성을 정의
  * unique : 유니크 속성을 정의
  * length : 컬럼의 길이를 지정
  * columnDefinition : 테이블의 속성을 직접 정의

## 5. @Enumerated

* Enum 타입을 컬럼에 매핑할때 사용.
  * EnumType.STRING : enum string을 value로 사용 (권장)
  * EnumType.ORDINAL : enum 순서값을 value로 사용 (비권장)

## 6. @Temporal
  * 날짜 관련 필드에 속성을 정의
  * LocalDate, LocalDateTime을 사용하는 경우 jpa에서 알아서 매핑
  * Date or Calendar 타입을 사용하는 경우, 아래 속성을 정의해야 함.
    * TemporalType.DATE : 날짜. dbms의 date 타입으로 매핑
    * TemporalType.TIME : 시간. dbms의 time 타입으로 매핑
    * TemporalType.TIMESTAMP : 날짜와 시간. dbms의 datetime 타입으로 매핑

## 7. Transient

* 특정컬럼을 매핑에서 제외하고자 할때 사용
