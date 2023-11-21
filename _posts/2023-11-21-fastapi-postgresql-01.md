---
title: "FastAPI + PostgreSQL DB 서버 만들기 (1) 프레임워크 선정 및 기반"
author: yoonkij
date: 2023-11-21 23:00:00 +0900
categories: [Dev, Side Projects]
tags: [api, database, fastapi, postgresql, series]
render_with_liquid: true
---

개인 프로젝트 진행 중 필요에 따라 다양한 소스에서 얻는 데이터를 적재할 수 있는 DB API 서버가 필요했다.
데이터베이스와 이를 서빙할 프레임워크의 선택지는 아주 많은데, 그 중 내가 선택한 조합은 FastAPI + PostgreSQL이다.

선택의 이유는 여러가지가 있는데, 가장 큰 이유는 두 가지이다:
(1) 빠른 구현이 가능할 것: 많은 사용자로부터의 대규모 트래픽을 받을 것이 아니고, 개인 프로젝트 + 데이터 적재용이며, 외부접속을 허용할 계획이 없기때문에 사용하지 않을 다양한 기능을 가진 프레임워크는 패스한다.
(2) 사용 안해본 + 남들은 많이 사용하는 기술일 것: 사이드 프로젝트는 학습을 위한 것이기도 하기때문에, 그동안 사용해보지 않지만 널리 사용되는 기술을 경험하고 싶었다.

# FastAPI

FastAPI는 이름에서 알 수 있듯이 Flask, Django보다 API를 만드는데에 더욱 집중된 프레임워크이다. 빠르고 간결한 코드로 서버를 만들 수 있고, API 개발에 필요한 입출력을 `Pydantic`으로 쉽게 정의 & 검증하고, 가장 인기있는 Python ORM 중 하나인 `Sqlalchemy`로 DB와 소통한다. 더불어 자동으로 생성해주는 Swagger API 문서는 많은 테스트작업을 단축시켜준다.

# PostgreSQL
기존에 경험이 있는 DB는 `mysql`과 `MongoDB` 이다. `mysql`은 RDBMS로 적절한 스키마 정의와 **간단한 CRUD, 읽기 작업에 두각**을 보이지만, 나는 CRUD 이외에 여러 JOIN과 쓰기작업이 필요하고, 이미 사용해본 적이 있기에 넘겼다.
`MongoDB`는 NoSQL의 대표주자로 이미 사용해본 점과 **자유로운 스키마가 초반 단계에는 장점이지만 시스템이 자리잡기 시작하면 일관성, 정합성을 파악하기가 어려운 단점**으로 제외했다.

NoSQL이 아닌 RDBMS + 사용은 해보지 않은 + 많이 사용되는 **`PostgreSQL`로 최종 결정**했다. PostgreSQL은 읽기-쓰기 동시성을 원활이 지원하고, 한번도 사용해보지 않았기에 이번 기회에 경험해보고자 한다.


# 디렉토리 구조
```bash
.
├── api
│   ├── __init__.py
│   ├── crud.py
│   └── routes.py
├── database
│   ├── __init__.py
│   ├── config.py
│   ├── models
│   │   ├── __init__.py
│   │   ├── instances.py
│   │   ├── primary.py
│   │   └── secondary.py
│   └── schemas
│       ├── __init__.py
│       ├── base.py
│       ├── instances.py
│       ├── primary.py
│       └── secondary.py
└── run.py
```
* `run.py`: uvicorn FastAPI 서버를 실행하는 간단한 스크립트
* `api`: CRUD API 관련 코드
	* `crud.py`: CRUD 동작을 구현한다.
	* `routes.py`: CRUD API endpoint를 정의한다.
* `database`: DB 설정 및 ORM 스키마 관련 코드
	* `models`: `sqlalchemy` DB 테이블 스키마 정의. PostgreSQL과 연결하여 테이블을 생성할 수 있다.
		* 한가지 주의할 점은 이미 없는 테이블을 만들어주는 것이라서, 테이블 컬럼 타입을 수정하면 UPDATE가 되지는 않는 것 같다.
	* `schemas`: `pydantic` 스키마. CRUD API 입출력 모델을 정의해서 쉽게 검증할 수 있다.

대략 구조는 위와 같다. 
다음에는 10개가 넘는 테이블을 차근차근 정의하고, 최대한 중복된 코드 없는 API를 만들어본다.

# 참고
1. [FastAPI](https://fastapi.tiangolo.com/ko/)
2. [PostgreSQL](https://www.postgresql.org/)
3. [PostgreSQL 사용하는 기업들](https://www.codenary.co.kr/techstack/detail/postgresql)
4. [Aurora MySQL vs Aurora PostgreSQL](https://techblog.woowahan.com/6550/)
5. [FastAPI (2) - PostgreSQL CRUD API 만들기](https://yihoeyiho.tistory.com/44)
6. ChatGPT
