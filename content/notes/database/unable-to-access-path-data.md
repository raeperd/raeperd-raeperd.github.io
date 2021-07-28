---
title: "Unable to Access Path Data"
date: 2021-02-21
tags: ["database", "bug"]
---

elasticsearch container 의 data path를 변경하면서 발견한 오류 

변경한 경로를 docker container 에 volumn mount 에도 설정해줘야한다.

해당 설정을 kibana 와 공유도 해줘야함 

```yml
version: '3'
services:
  elastic:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.9.1
    environment:
      #ES_HEAP_SIZE: 16g
      path.data: /home/raecheol-park/elastic/data
      path.logs: /home/raecheol-park/elastic/logs
      NODE_OPTIONS: --max_old_space_size=4096
      ES_JAVA_OPTS: -Xms2G -Xmx2G
      discovery.type: single-node
      xpack.security.enabled: "true"
      ELASTIC_PASSWORD: elastic1234
    ports:
     - "9201:9200"
    volumes:
      - esdata:/home/raecheol-park/elastic/data
      - eslogs:/home/raecheol-park/elastic/logs

  kibana:
    image: docker.elastic.co/kibana/kibana:7.9.1
    environment:
      ELASTICSEARCH_HOSTS: http://elastic:9200
      ELASTICSEARCH_USERNAME: elastic
      ELASTICSEARCH_PASSWORD: elastic1234
      #xpack.reporting.csv.maxSizeBytes: 304857600
    ports:
      - "5602:5601"

volumes:
  esdata:
  eslogs:
```

[Unable to access 'path.data' · Issue #74 · docker-library/elasticsearch](https://github.com/docker-library/elasticsearch/issues/74#issuecomment-166768744)