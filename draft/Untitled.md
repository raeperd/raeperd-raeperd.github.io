# 문제원인

azure postgres 의 url table의 sync 가 안맞음으로 인해 생긴 문제 

- 주된 원인은 insert 상에서 생긴 conflict 등이 원인이라고 하는데 정확한 원인 파악은 어려움
- 이전 db에서 오류가 발생해 복원한 적이 있는데 이 이슈가 그 원인일 수도 있음



# 해결방안 / 수정 사항

```
SELECT MAX(url.id) FROM url;
SELECT nextval('url_id_seq');
```

이 두 쿼리의 결과에서 앞의 쿼리의 결과의 값이 더 크다면 테이블의 sync가 망가진 것

실제로 배포 환경에서 각각 12448, 1355의 값이 나옴



아래의 쿼리로 해결할 수 있었다.

```
SELECT setval('url_id_seq', (SELECT MAX(url.id) FROM url) + 1);
```



# 참고

https://stackoverflow.com/questions/4448340/postgresql-duplicate-key-violates-unique-constraint

https://hcmc.uvic.ca/blogs/index.php/how_to_fix_postgresql_error_duplicate_ke?blog=22