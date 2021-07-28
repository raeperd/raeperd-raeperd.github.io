---
title: "Dataclass Default Factory"
date: 2020-11-02
tags: ["python"]
---

python에서 dataclass 의 초기값으로 container나 따로 정의한 class와 같이, non-literal valure 를 사용할 경우 오류가 생길 수 있다.

default_factory를 활용해 초기화 하면 이를 피할 수 있다.

```python
@dataclass
class CommandStruct():
    Command                 : int = COMMAND_INIT
    SampleFile              : str = ""
    MaxDynamicAnalysisCount : int = 3
    PlatformType            : int = PLATFORM_TYPE
    Ticket                  : Ticket= dataclasses.field(default_factory=Ticket)

@dataclass
class Ticket:
    TicketID: str = ""
    OriginalFileName: str = ""
    SampleFile: str = ""
    ResultDir: str = ""
    ReportFile: str = ""
    Attributes: Dict[str, Any] = dataclasses.field(default_factory=lambda: {})
```

데이터 클래스가 중첩되어 있는 상황에서 오류를 찾기 힘들었다.

커버리지 100을 목표로 테스트 하다가 발견한 오류

역시 테스트는 많이하면 할 수록 좋다 .