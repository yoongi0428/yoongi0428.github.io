---
title: "메세지 큐 도입기: RabbitMQ, Kafka"
author: yoonkij
date: 2023-08-26 09:00:00 +0900
categories: [Dev, DevOps]
tags: [devops, message-queue]
render_with_liquid: true
---
### 도입 배경

주식, 코인 관련 개인 프로젝트를 진행 중 잦은 구조 변경과 환경 변경으로 인해
컴포넌트 일부를 멈추고 수정하고 재개하는 일이 잦았다.
빠르게 하다보니 주먹구구식 스파게티가 많아진 덕지덕지의 결과인데,
이제는 정리해보자.
최대한 역할에 따라 개별 컴포넌트를 분리하기 해보자.
그 시작으로 메세지 큐를 도입하여 데이터 생산자와 소비자를 나눈다.
들어봤고 많이 사용하는 RabbitMQ & Kafka를 간략하게만 조사해보자

### 목표
- 거래소 API, 웹크롤링 등을 통해 필요한 정보를 얻고 후처리를 통해 변환한 후 DB에 저장하는 로직
- 후처리 로직을 수정, 변경 혹은 일시적인 셧다운을 할 경우 전체 로직을 멈춰야하는 경우가 발생
- 간단한 메세지 큐 도입을 통해 Producer, Consumer를 분리하고 데이터 누수를 막아보자

### RabbitMQ & Kafka

- 우선 내게 필요한 스펙은
    - 실시간 스트림 X
    - 개별 데이터 크기 아주 작음 (텍스트와 숫자 몇개)
    - 수도 적음 (30분마다 20개, 6시간마다 1개, 간헐적으로 몇개씩 등)

- RabbitMQ는 브로커를 통한 전통적인 메세지 생산-소비 구조
    - producer→ consumer queue binding 지정
    - 소비 후 메세지 삭제
    - multiple queue
- Kafka는 스트림
    - producer → consumer로 직접 배달하지 않음
    - producer → 토픽, 파티션 → consumer
    - producer는 파티션에 진열할 뿐 consumer가 가져가는지는 모름

나는 대용량, 대규모 분산처리가 필요한 실시간 스트림 데이터가 아니고

단순 메세지 큐만 필요하고

이러한 시스템들에 입문하기 위한 비교적 간단한 툴을 원하기에

**RabbitMQ**로 간다

### Reference

- https://chat.openai.com/share/b516d98d-382a-42e3-a79d-7607ca781068
- https://aws.amazon.com/ko/compare/the-difference-between-rabbitmq-and-kafka/
- https://escapefromcoding.tistory.com/705