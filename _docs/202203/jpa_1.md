---
title: jpa 테이블 필드 매핑 애노테이션
category: java
order: 9998
---

## 1. @Entity

* @Entity가 붙은 클래스는 jpa가 관리.
* 기본생성자 필수 (파라미터가 없는 public or protected 생성자)

## 2. @Table

* 겍체와 매핑할 DBMS의 테이블 이름을 정의.
* 기본값은 entity 이름(클래스)
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
