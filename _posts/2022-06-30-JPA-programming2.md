---
layout: post
title: "자바 ORM 표준 JPA 프로그래밍 - 기본편"
excerpt: "자바 ORM 표준 JPA 프로그래밍 강의노트 요약입니다."
subtitle: "JPA"
toc: true
toc_sticky: true
toc_label: "페이지 주요 목차"
date: 2022-6-30
tags: [JPA]
---
### 다양한 연관관계 맵핑

  - 연관관계 맵핑 시 고려할 사항
    - 다중성: @ManyToOne, @OneToMany, @OneToOne, @ManyToMany (거의 안쓰임)
    - 단방향, 양방향
    - 연관관계의 주인
      - 객체의 양방향 관계는 A->B, B->A처럼 참조가 2군데이며, 둘 중 테이블의 외래 키를 관리할 곳을 지정
      - 외래 키를 관리하는 참조가 연관관계의 주, 주인의 반대편은 읽기만 가능

#### N:1 [일대일]

  - 주 테이블에 외래 키
    - 주 객체가 대상 객체의 참조를 가지는 것처럼 주 테이블에 외래 키를 두고 테이블을 찾음
    - JPA 맵핑 편리, 주 테이블만 주회해도 대상 테이블에 데이터가 있는지 확인 가능하나 값이 없으면 외래 키에 null 허용해야 함

  - 대상 테이블에 외래 키
    - 대상 테이블에 외래 키가 존재
    - 주 테이블과 대상 테이블을 1:1에서 1:N 관계로 변경하게 될 때 테이블 구조를 유지할 수 있음
    - 프록시 기능의 한계로 **지연 로딩으로 설정해도 항상 즉시 로딩** 
  
#### 1:N [일대다]

  - 단방향
    - 엔티티가 관리하는 외래키가 다른 테이블에 있음
    - 연관관계 관리를 위해 추가로 UPDATE SQL 실행
    - 일대다 단방향 맵핑은 지양하고 **다대일 양방향**을 지향

  - 양방향
    - 공식적으로 존재 X
    - @JoinColumn(insertable=false, updatable=false)
    - 읽기 전용 필드를 사용해서 양방향 처럼 사용하는 얍삽이 방법 존재
  
#### N:M [다대다]

- 관계형 DB는 정규화된 테이블 2개로 해당 관계를 표현할 수 없음
- 연결 테이블을 추가해서 일대다, 다대일 관계로 풀어내야 함
- 객체는 다대다 구현이 가능하나 실무에서는 거의 안 쓰임


### 고급맵핑

#### 상속관계 맵핑

- 관계형 DB는 상속 관계 X
- 슈퍼타입 서브타입 관계라는 모델링 기법이 객체 상속과 유사 -> 객체의 상속 및 구조와 DB의 슈퍼타입 서브타입 관계를 맵핑
- 논리 모델 vs 물리 모델

- 조인 전략: 필요한 데이터를 조인으로 가져오기 