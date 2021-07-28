# Git remote branch

Category: git
Created: November 2, 2020 9:08 PM
Created By: RaeCheol Park
Last Edited Time: November 2, 2020 9:08 PM
Status: DONE

원격 저장소 브랜치 확인

```bash
 $ git branch -r
```

원격과 로컬 모든 브랜치 확인

```bash
 $ git branch -a 
```

원격 저장소의 브랜치로 checkout

```bash
 $ git checkout -t origin/feature/some-branch
```

option이 왜이렇게 일관성이 없을까?
원격 브랜치를 확인할때는 `-r` 이지만 checkout 할때는 `-t` 를 쓰라고? 매번 확인하게 만드네

더 자세한 옵션은 `--help` 를 추가하고 명령을 하면 html 형식의 매뉴얼을 확인할 수 있다.