---
title: "데이터베이스 교체: MongoDB & PostgreSQL"
author: yoonkij
date: 2023-08-28 09:00:00 +0900
categories: [Dev, DevOps]
tags: [devops, database]
render_with_liquid: true
---
지난번 메세지 큐 도입과 함께 주먹구구 스파게티 개인프로젝트 코드를 정리하며 데이터베이스에 대해서도 다시 생각해보았다.

지금까지는 MongoDB Atlas 를 사용해왔다. 그 이유는

1. 스키마에 대한 고민을 할 시간에 빠르게 구현하여 확인하고 싶었고
2. 초기에 많은 스키마 변경이 있을 것 같았고
3. 딱히 join 오퍼레이션은 필요하지 않았으며
4. 데이터를 그대로 push 할 수 있고
5. Atlas 클라우드를 통해 개인 장비, AWS 등 여러 곳에서 (IP 화이트리스팅만 하면) 자유롭게 접근이 가능했기 때문이다.

어느 정도, 컨셉 검증이 끝나고 본격적인 운영 코드로 정리하고 싶은 이 시점에는 많은 변화가 있어 다시 생각해볼 필요가 있다. 달라진 점은,

1. 클라우드 → 맥미니 홈서버로 전환했기 때문에, 클라우드 DB가 더이상 필요 없고
2. 대략적인 스키마가 완성되었으며
3. 그에 따라 DB 정규화 도입, join 오퍼레이션으로 로지컬한 테이블 정의가 이제 가능해졌다.

### MongoDB와 PostgreSQL

|  | MongoDB | PostgreSQL |
| --- | --- | --- |
| 데이터모델 | 도큐먼트 (K-V) | 객체 관계형 |
| 스키마 | Dynamic | Predefined |
| 일관성, 무결성 | X | O |
| 유연성 | O | X |
| 쿼리 | MongoDB QL | SQL-like |
| Join | X | O |

MongoDB를 그동안 사용하면서 느낀 장단점은 아래와 같다.

**장점**

- 스키마가 자유롭다: 특별히 데이터타입과 테이블 스키마, 테이블간의 관계를 고민할 필요 없이, 필요한 정보를 Key-Value로 묶어 밀어 넣으면 된다. 심지어는 Nested 도 가능하기 때문에 Python 딕셔너리라고 생각하면 된다.
- 개인적으로 느끼기에 초기 러닝커브가 낮아 빠른 프로토타입에 사용하기 좋았다.

**단점**

- 스키마가 자유롭다: 어느 날, 새 도큐먼트를 넣을 때 카멜룰로 써놓은 컬럼명을 잘못하여 스네이크로 써서 넣었는데 (newColumn → new_column), 오류가 나지않아 일관성이 깨진 경우가 있었다. 나중에야 발견하고 급히 수정했다.
- Join없이 모든 데이터를 넣기 때문에, 덕지덕지 패치하다가는 스파게티가 된다.

큰 데이터가 아니고, 쿼리나 인덱싱을 많이 사용하지 않아 이 부분은 충분히 파악이 되지 않았다.

처음에는 스키마리스가 장점이었으나, 개발을 진행하면서 단점이 크게 느껴졌고, 깔끔하고 톱니바퀴처럼 돌아가는 시스템을 좋아하는 사람으로써 정해진 룰을 자꾸 찾게됐다. 어느 정도 스키마 틀이 잡힌 단계에 오니 더더욱 그러했다.

다른 관계형 DB도 많으나 오픈소스인 PostgreSQL을 사용하고 뜯어보며 개발 생태계에 가능하면 기여해보고자 한다.

**PostgreSQL !!**

### Reference

1. https://aws.amazon.com/ko/compare/the-difference-between-mongodb-and-postgresql/
2. https://bitnine.tistory.com/48
3. https://heekangpark.github.io/etc/mysql-vs-postgresql-vs-mongodb