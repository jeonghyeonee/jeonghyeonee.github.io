---
layout: post
title: 가상 면접 사례로 배우는 대규모 시스템 설계 기초 Chap1; 사용자 수에 따른 규모 확장성
categories: systemdesign-interview
lang: ko
lang-ref: systemdesign-interview-chap1
tags: [SystemDesign, Interview]
---

## Chap1. 사용자 수에 따른 규모 확장성

> 목표 <br>
> 한 명의 사용자를 지원하는 시스템에서 시작하여, 최종적으로는 몇백만 사용자를 지원하는 시스템을 설계

## 단일 서버

- 모든 컴포넌트가 단 한대의 서버에서 실행되는 시스템
- 사용자 요청 처리 흐름

  1.  사용자는 도메인 이름(api.mysite.com)을 이용하여 웹사이트에 접속
      도메인 이름 서비스(Domain Name Service, DNS)에 질의하여 IP 주소로 변환하는 과정 필요
      DNS는 보통 제3사업자(third party)가 제공하는 유료 서비스 이용 → 우리 시스템 일부 X - DNS - References
      [https://hanamon.kr/dns란-도메인-네임-시스템-개념부터-작동-방식까지/](https://hanamon.kr/dns%EB%9E%80-%EB%8F%84%EB%A9%94%EC%9D%B8-%EB%84%A4%EC%9E%84-%EC%8B%9C%EC%8A%A4%ED%85%9C-%EA%B0%9C%EB%85%90%EB%B6%80%ED%84%B0-%EC%9E%91%EB%8F%99-%EB%B0%A9%EC%8B%9D%EA%B9%8C%EC%A7%80/)

              - Domain Name System
              - 도메인 이름을 IP 주소로 변환하는 시스템
              - 전세계적으로 약속된 규칙 공유
              - 상위 기관에서 인증된 기관에게 도메인을 생성 or IP주소로 변경할 수 있는 ‘권한’을 부여
              - 계층구조(상위 기관, 하위 기관)를 가지는 분산 데이터베이스 구조

              **구성요소**

              - Domain Name Space
                  - 도메인 이름 저장을 분산
              - Name Server
                  - 권한 있는DNS 서버
                  - 해당 도메인 이름의 IP 주소 찾기
              - Resolver
                  - 권한 없는 DNS 서버
                  - DNS 클라이언트 요처을 네임 서버로 전달
                  - 찾은 정보를 클라이언트에게 제공

  2.  DNS 조회 결과로 IP 주소 반환
      e.g. [api.mysite.com](http://api.mysite.com) → 15.120.23.214(웹 서버의 주소)
  3.  해당 IP 주소로 HTTP(HyperText Transfer Protocol) 요청 전달
  4.  요청을 받은 웹 서버는 HTML 페이지나 JSON 형태의 응답 반환

  - 실제 요청은 어디서?
    - 웹 애플리케이션
      - 비지니스 로직, 데이터 저장 등을 처리하기 위해서 서버 규현용 언어(Java, Python …)를 사용
      - 프레젠테이션둉으로는 클라이언트 구현용 언어(HTML, JavaScript, …)를 사용
    - 모바일 앱
      - 모바일 앱과 웹 서버 간 통신을 위해서 HTTP 프로토콜을 이용
      - HTTP 프로토콜을 통해 반환될 응답 데이터의 포맷으로는 보통 JSON(JavaScript Object Notation)이 간결하여 많이 쓰임

## 데이터베이스

- 사용자가 늘어나면 서버 하나로 충분하지 않음 → 여러 서버 필요
- 웹/모바일 트래픽 처리 용도 + DB용
- Web/Mobile 트래픽 처리 서버(웹 계층) + 데이터베이스 서버(데이터 계층)를 분리
  → 독립적으로 확장 가능

### 어떤 데이터베이스를 사용할 것인가?

- Relational Database와 비-관계형 데이터베이스 사에엇 고를 수 있음
- 관계형 데이터베이스
  - RDBMS(Relational Database Management System)
  - MySQL, Oracle DB, PostgreSQL
  - 테이블과 열과 칼럼으로 자료를 표현
  - SQL을 사용하면 여러 테이블에 있는 데이터를 그 관계에 따라 조인하여 합칠 수 있음
- 비관계형 데이터베이스
  - NoSQL
  - CouchDB, Neo4j, Cassandra, HBase, Amazon, DynamoDB
  - key-value store
  - graph store
  - column store
  - join 연산은 지원하지 않음
  - NoSQL이 바람직한 선택일 경우
    - 아주 낮은 응답 지연시간(latency)이 요구됨
    - 다루는 데이터가 비정형(unstructured)이라 관계형 데이터가 아님
    - 데이터(JSON, YAML, XML 등)를 직렬화하거나(serialize) 역직렬화(deserialize)할 수 있기만 하면 됨
    - 아주 많은 양의 데이터를 저장할 필요가 있음

## 수직적 규모 확장 vs. 수평적 규모 확장

### Scale Up: 수직적 규모 확장(vertical scaling)

- 서버에 고사양 자원을 추가하는 행위
  - e.g. 더 좋은 CPU, 더 많은 RAM …
- 서버로 유입되는 트래픽의 양이 적을 때
- 장점
  - 단순함
- 단점
  - 수직적 규모 확장에는 한계가 잇음
    - 한 대의 서버에 CPU나 메모리를 무한대로 증설할 방법은 없음
  - 장애에 대한 자동복구(failover)방안이나 다중화(redundancy) 방안을 제시하지 않음
    - 서버에 장애 발생 → 웹사이트/앱은 완전히 중단
      ⇒ 대규모 애플리케이션을 지원하는 데는 수평적 규모 확장이 적절

### Scale Out: 수평적 규모 확장

- 더 많은 서버를 추가하여 성능 개선
- 대규모 애플리케이션 지원

> ✨ 예시 구조에 여전히 남아있는 **문제점**
>
> - 웹 서버 다운 → 사용자 웹 사이트 접속 불가
> - 너무 많은 사용자 접속 → 웹 서버 한계 도달 → 응답 속도 느려짐 or 서버 접속 불가  
>   ⇒ 부하 분산기 or load balancer 도입

### 로드밸런서

- load balancing set(부하 분산 집합)에 속한 웹 서버들에게 트래픽 부하를 고르게 분산하는 역할
- 로드밸런서의 public IP address로 접속 → 웹 서버는 클라이언트 접속을 직접 처리 X
- 서버 간 통신 → private IP address가 이용
  - 같은 네트워크에 속한 서버 사이의 통신에만 쓰일 수 있는 IP address
  - 인터넷을 통해서 접속 불가
  - 웹 서버와 통신하기 위해 private IP address 이용
- 문제점 해결
  - 부하 분산 집합에 또 하나의 웹 서버 추가 → no failover(장애를 자동복구하지 못하는 문제) 해소
  - 웹 계층 가용성(availability) 향상
  - 서버 1 다운(offline) → 모든 트래픽 서버 2로 전송
    ⇒ 웹 사이트 전체가 다운되는 일이 방지
    ⇒ 부하를 나누기 위해 새로운 서버 추가 가능
  - 웹사이트로 유입되는 트래픽이 가파르게 증가 → 두 대의 서버로 트래픽을 감당할 수 없는 시점 발생 → 로드밸런서가 있으므로 우아하게 대처할 수 있음
    ⇒ 웹 서버 계층에 더 많은 서버를 추가 → 로드밸런스가 자동적으로 트래픽 분산 시작

> ✨ 예시 구조에 여전히 남아있는 **문제점**
>
> - 웹 계층은 OK~! 그런데! 데이터 계층은????
> - 하나의 DB 서버만 존재
> - 장애의 자동복구나 다중화를 지원하는 구성 X  
>   ⇒ 데이터베이스 다중화

### 데이터베이스 다중화

- 많은 데이터베이스 관리 시스템이 다중화를 지원함
- 서버 사이에서 master(주)-slave(부) 관계 설정
- master
  - 데이터 원본
  - write operation
- slave
  - 데이터 사본
  - master DB로부터 사본을 전달받음
  - read operation
  - DB변경 명령어(insert, delete, update 등) → master로 전달
  - 읽기 연산의 비중이 쓰기 연산보다 훨씬 높음
  - master DB보다 slave DB의 수가 더 많음
- 장점
  - 더 나은 성능
    - master-slave 다중화 모델에서 모든 데이터 변경 연산은 master DB로만 전달됨
    - read operation은 slave DB 서버들로 분산됨
    - 병렬로 처리될 수 있는 질의(query)의 수가 늘어남 → 성능 향상
  - Reliability(안정성)
    - 자연 재해 등의 이유로 DB 서버 가운데 일부 파괴 → 데이터 보존 O
      - WHY? 지역적으로 떨어진 여러 장소에 다중화시켜놓을 수 있기 때문
  - Availability(가용성)
    - 데이터를 여러 지역에 복제 → 하나의 DB 서버에 장애 발생 → 다른 서버에 있는 데이터를 가져와 계속 서비스 O
- 문제점 해결
  - slave 1대 case
    - 다운 → 읽기 연산은 한시적으로 모두 master DB로 전달
    - 새로운 slave DB 서버가 장애 서버를 대체할 것
