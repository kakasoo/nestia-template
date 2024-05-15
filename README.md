# Project Tech Stack

- NestJS
- Nestia & Typia
- Prisma
- Postgres
- Docker

# docker-compose

```
$ docker compose up -d
```

현재는 TEST DB를 생성하여 로컬에서 작업할 수 있도록 돕는 역할만을 수행합니다.  
진행 시 [`init.sql`](./init.sql)을 실행하여 데이터베이스의 생성까지 수행합니다.

# Prisma

## Environment

Prisma는 .env에서 DATABASE_URL을 가져다가 사용하게끔 되어 있습니다.  
현재는 docker-compose로 올린 TEST DB만 존재하기 때문에 아래 값을 고정으로 사용합니다.  
필요 시 동료에게 `DATABASE_URL`을 요청해서 받으세요.  
아래는 로컬에서 사용 가능한 TEST용 DB의 값이며, 로컬 개발 시 사용해도 되지만 개발이나 프로덕션에서는 사용할 수 없습니다.

```bash
DATABASE_URL="postgresql://postgres:password@localhost:6543/template?schema=public"
```

## 주요 명령어

```bash
$ npx prisma db push
```

schema.prisma 파일에 설정된 스키마 정보를 토대로 DB 내 테이블을 생성합니다.  
이미 데이터가 있거나 테이블 수정 시 데이터의 정합성을 보장할 수 없는 경우 데이터를 삭제할 것인지 묻는 문장들이 나옵니다.  
이 때 `Y`를 입력하면 테이블의 row가 삭제되므로 주의해주시고, TEST DB 외에 사용은 금합니다.

```bash
$ npx prisma generate
```

schema.prisma 설정대로 node_modules 내 타입들을 생성합니다.  
모든 Prisma 프로젝트는 위 명령어를 사용하여 DB에 맞게 `prisma/client`를 생성해야 합니다.  
생성하지 않으면 타입추론이 전혀 되지 않아 사용할 수가 없는 상태입니다.

```bash
$ npx prisma db pull
```

DB 정보를 토대로 schema.prisma 파일을 생성합니다.  
우리는 git으로 schema 파일을 공유하기 때문에 굳이 사용할 필요가 없습니다.  
또한 Prisma는 DB 내 주석을 허용하지 않기 때문에 괜히 schema 파일에 명시한 주석만 날라갈 수 있으니 주의해야 합니다.
