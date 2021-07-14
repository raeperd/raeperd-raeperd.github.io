---
title: "Duplicate Key Value Violates Unique Constraint Pkey"
date: 2020-11-09
tags: ["database", "bug"]
---

```
postgres.public> insert into url(created_date,address,vendor_id, malicious) values('2020-11-09 04:29:46.438652','http://pim-square.com/Korea/',2, True)
[2020-11-09 13:38:29] [23505] ERROR: duplicate key value violates unique constraint "url_pkey"
[2020-11-09 13:38:29] Detail: Key (id)=(1351) already exists.
```

`AUTO INCREMENT`로 설정된 PRIMARY KEY 는 `INSERT` 할 때 명시하지 않아도 알아서 삽입되어야 하는데 중복이 됬다고 오류가 발생함. 

로컬의 DB에서는 정상 동작하는 것으로 보아 query나 db constraint 같은 것이 원인은 아니는 듯 했다.

아래 링크에서 원인을 찾을 수 있었는데, db 의 sync가 나가서 생기는 문제라고 한다. 

[postgresql duplicate key violates unique constraint](https://stackoverflow.com/questions/4448340/postgresql-duplicate-key-violates-unique-constraint)

[How to fix PostgreSQL error "duplicate key violates unique constraint"](https://hcmc.uvic.ca/blogs/index.php/how_to_fix_postgresql_error_duplicate_ke?blog=22)  

```sql
SELECT MAX(url.id) FROM url;
SELECT nextval('url_id_seq')
```

이런식으로 확인했을때, 앞의 query의 결과가 뒤의 query의 결과보다 크다면 sync가 나간것

실제로 해보니까 이렇게 나옴

```
12448
1355
```

# How to solve
```sql
SELECT setval('url_id_seq', (SELECT MAX(url.id) FROM url) + 1);
```
