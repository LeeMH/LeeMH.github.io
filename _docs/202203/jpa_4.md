---
title: 상속관계 매핑
category: java
order: 9995
---


## 1. 상속관계 DB 구현 방법

* 서브 테이블
  * 공통 데이터와 개별 데이터 테이블을 각각 구성
  * 공통 테이블에는 어떤 서버 테이블과 join 되어야 하는지 구분하는 필드(DTYPE) 필요
  * 장점
    * 데이터가 정규화 되는 장점
  * 단점
    * 조회시 join이 필수
* 통합 테이블
  * 공통 데이터와 개별 데이터의 모든 집합을 하나의 테이블로 구성
  * 개별 row가 어떠한 속성의 데이터인지 구분하는 필드(DTYPE) 필요
  * 장점
    * join이 필요없으며, 쿼리가 단순
  * 단점
    * 자식 엔티티가 매핑한 컬럼은 모두 null 허용필수
* 개별 테이블
  * 별도의 테이블로 구성
  * 조인전략
  * 추천안함!!


## 2. @OneToOne

* 일대일 연관관계 설정
* 다대일과 동일하게, 연관관계의 주인은 @JoinColumn 애노테이션을 가지고,
* 반대쪽은 @MappedBy 애노테이션을 가진다.

