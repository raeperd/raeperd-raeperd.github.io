# 현상

m1 mac 에서 개발된 이미지를 실행해보면 아래와 같은 오류를 확인할 수 있음

![fff0ce23-f202-4e34-b512-ebeb77a77dbf](https://i.ibb.co/VTfrnVz/fff0ce23-f202-4e34-b512-ebeb77a77dbf.png)



# 원인

현재 버전에서 threat-intelligence service가 생산하고 있는 컨테이너의 base image는 adoptopenjdk/openjdk11:jdk-11.0.9.1_1-alpine-slim 로 amd 아키텍쳐만을 지원한다.



# 개선 방안

m1 mac 과 같이 arm 기반 아키텍처에서 동작하도록 하려면 두 플랫폼 모두를 지원하는 베이스 이미지로부터 컨테이너 이미지를 생산해야 한다. gradle jib 에서도 이런 동작을 지원한다.

후보들 몇가지

docker pull openjdk:11.0.10-jre-slim-buster

docker pull openjdk:11.0.10-jre-slim

docker pull openjdk:11.0.10-jre-buster

참고)

[Dockerhub/OpenJDK](https://hub.docker.com/_/openjdk?tab=description&page=1&ordering=last_updated&name=11)

[Alpine, Slim, Stretch, Buster, Jessie, Bullseye — What are the Differences in Docker Images?](https://medium.com/swlh/alpine-slim-stretch-buster-jessie-bullseye-bookworm-what-are-the-differences-in-docker-62171ed4531d)

[Gradle-jib-plugin-FAQ how-do-i-specify-a-platform-in-the-manifest-list-or-oci-index-of-a-base-image](https://github.com/GoogleContainerTools/jib/blob/master/docs/faq.md#how-do-i-specify-a-platform-in-the-manifest-list-or-oci-index-of-a-base-image)