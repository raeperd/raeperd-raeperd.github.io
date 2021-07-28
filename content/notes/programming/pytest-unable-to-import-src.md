---
title: "Pytest Unable to Import Src"
date: 2020-04-21T03:27:24+09:00
tags: ["python", "bug"]
---

pytest를 커맨드라인에서 실행할때 현재 경로를 PYTHONPATH에 추가하지 않기 때문에 생기는 오류다. pytest를 사용할때마다 겪는 오류 같다.

패키지를 테스트할때는 [setup.py](http://setup.py) develop 과 같은 명령으로 패키지를 설치하는 것을 강제하도록 테스트를 작성하는게 유용할 것. 하지만 간단한 작업을 하는 스크립트들은 그럴 필요가 없다.

프로젝트 루트에 [conftest.py](http://conftest.py) 를 추가하는 것으로 가능하다.

[PATH issue with pytest 'ImportError: No module named YadaYadaYada'](https://stackoverflow.com/a/50610630)

pycharm 같은 친구들은 자동으로 처리해줘서 테스트가 되나보다. 그래서 배포환경에서도 테스트를 한번 더 실행해볼 필요가 있는 듯 

추가로 참고할 만한 것

[Packaging a python library - Thoughts on packaging python libraries](https://blog.ionelmc.ro/2014/05/25/python-packaging/#the-structure)

[Good Integration Practices - pytest documentation](https://docs.pytest.org/en/stable/goodpractices.html)

---

- 사실 요즘에는 python 프로젝트에서는 src 와 같은 자명한 폴더를 만들지 않는게 좋다는 결론을 얻음