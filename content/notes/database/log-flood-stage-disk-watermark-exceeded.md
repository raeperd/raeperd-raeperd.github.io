---
title: "Log Flood Stage Disk Watermark Exceeded"
date: 2020-10-22
tags: ["database", "docker", "bug"]
---

# 현상
```
Log flood stage disk watermark [95%] exceeded on – How To Solve Related Issues
```

# 해결방안

df 명령을 쳐보면 어디에 쓰고 있는지 보인다.

```python
raecheol-park@dev:~/docker-elk$ df -h 
Filesystem      Size  Used Avail Use% Mounted on
udev             16G     0   16G   0% /dev
tmpfs           3.2G  339M  2.8G  11% /run
/dev/sda2       438G  398G   18G  96% /
tmpfs            16G     0   16G   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs            16G     0   16G   0% /sys/fs/cgroup
/dev/md0        1.8T  876G  865G  51% /working
/dev/sda1       511M  3.4M  508M   1% /boot/efi
tmpfs           3.2G     0  3.2G   0% /run/user/5002
tmpfs           3.2G     0  3.2G   0% /run/user/1002
```

host의 mount 경로를 명시하지 않으면 기본 경로에 mount 를 하게 되는데 이 경로의 사용량이 95% 이상이 되면 read only 모드로 예약된 부분을 쓰기 권한으로 사용할 수 없으므로 오류가 발생한다. 

다른 경로에, 디스크가 여유분이 있는 경로를 직접 명시하면 이런 문제를 피할 수 있다 .

```yaml
version: '3'
services:
  elastic:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.9.1
    environment:
      #ES_HEAP_SIZE: 16g
      ES_JAVA_OPTS: -Xms2G -Xmx2G
      discovery.type: single-node
      xpack.security.enabled: "true"
      ELASTIC_PASSWORD: changeme
    ports:
     - "9201:9200"
    volumes:
      - /working/raecheol-park/elastic/data:/usr/share/elasticsearch/data

  kibana:
    image: docker.elastic.co/kibana/kibana:7.9.1
    environment:
      ELASTICSEARCH_HOSTS: http://elastic:9200
      ELASTICSEARCH_USERNAME: elastic
      ELASTICSEARCH_PASSWORD: changeme
      #xpack.reporting.csv.maxSizeBytes: 304857600
    ports:
      - "5602:5601"
```