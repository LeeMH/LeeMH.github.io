---
title: 즉시로딩, 지연로딩, 영속성 전이
category: java
order: 9994
---


## 1. jpa 실무 관점 즉시로딩, 지연로딩

* 가능한 지연 로딩만 사용
  * 즉시 로딩을 사용하면, 예상하지 못한 SQL이 발생
* 즉시 로딩은 N+1 문제를 야기
  * findById등 PK select 가 아닌경우, 주된 entity를 조회하고
  * 즉시 로딩이 걸린 entity들을 순회하며 select를 수행
* @ManyToOne, @OneToOne은 기본이 즉시 로딩 -> FetchType.LAZY로 설정
* @OneToMay는 지연로딩이 디폴트
* 지연로딩을 기본으로 하되, 즉시로딩이 필요한 경우 아래 2가지로 접근
  * JPQL fetch join
  * entity graph


## 2. 영속성 전이(cascade)

* 
* entity를 저장할때, 연관관계가 있는 entity 들까지 포함해서 처리한다.
* 예를들어 부모(1), 자식(N)의 관계가 있을때, 부모 entity 저장시 자식 entity까지 함께 처리를 원하는 경우
* CascaseType 옵션을 사용
  
