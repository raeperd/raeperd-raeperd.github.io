# gradle JaCoCo coverage configuration

Category: gradle
Created: November 10, 2020 6:30 PM
Created By: RaeCheol Park
Last Edited Time: November 10, 2020 6:30 PM
Status: NEED_DETAIL

plugin settings

```groovy
plugins {
    id 'org.springframework.boot' version '2.3.3.RELEASE'
    id 'java'
    id 'jacoco'
}
```

task settings

```groovy
task unitTest(type: Test) {
    useJUnitPlatform {
        excludeTags("integration")
    }
}

test {
    useJUnitPlatform()
    finalizedBy 'jacocoTestReport'
}

jacocoTestReport {
    reports {
        html.enabled true
    }
    finalizedBy 'jacocoTestCoverageVerification'
}

jacocoTestCoverageVerification {
    violationRules {
        rule {
            element = 'PACKAGE'

            limit {
                counter = 'CLASS'
                value = 'COVEREDRATIO'
                maximum = 1
            }

            excludes = [
            ]
        }
        rule {
            element = "CLASS"

            limit {
                counter = 'LINE'
                value = 'TOTALCOUNT'
                maximum = 100
            }
            limit {
                counter = 'METHOD'
                value = 'COVEREDRATIO'
                minimum = 1
            }

            excludes = [
            ]
        }
        rule {
            element = "METHOD"

            limit {
                counter = 'INSTRUCTION'
                value = 'COVEREDRATIO'
                minimum = 1
            }

            excludes = [
            ]
        }
        rule {
            element = "METHOD"

            limit {
                counter = 'LINE'
                value = 'TOTALCOUNT'
                maximum = 10
            }

            excludes = [
            ]
        }
    }
}
```

referenced 

[Gradle 프로젝트에 JaCoCo 설정하기 - 우아한형제들 기술 블로그](https://woowabros.github.io/experience/2020/02/02/jacoco-config-on-gradle-project.html)