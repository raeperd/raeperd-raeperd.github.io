# File allocation methods

### Contiguous allocation

- Single contiguous set of blocks is allocated to a file at file creation time
- External fragmentation can occur in case of dynamic creation & deletion of files

### Chained allocation: non-contiguous allocation

- Each block contains a pointer to the next block in chain
- No external fragmentation
- Adequate for dynamic creation & deletion of files

### Indexed allocation: non-contiguous allocation

- Index block has data block list for a file
- No external fragmentation
- Adequate for dynamic creation & deletion of files

## Contigous allocation

![File%20allocation%20methods%208a38c0fe57a14a6694b1e82c2075060e/Untitled.png](File%20allocation%20methods%208a38c0fe57a14a6694b1e82c2075060e/Untitled.png)

- 하드디스크가 느린 것은 랜덤 접근이 어렵기 때문이다.
- 한번 접근하면 빠르게 읽을 수 있다.

![File%20allocation%20methods%208a38c0fe57a14a6694b1e82c2075060e/Untitled%201.png](File%20allocation%20methods%208a38c0fe57a14a6694b1e82c2075060e/Untitled%201.png)

- 그런데 파일을 삭제할 경우 fragmentation 이 일어난다.
- 총 11개의 공백이 있지만 7개 짜리 블록을 만들 수 없다.
- CD-ROM의 경우 이런식으로 이루어져 있다.

## Chained allocation: non-contiguous allocation

![File%20allocation%20methods%208a38c0fe57a14a6694b1e82c2075060e/Untitled%202.png](File%20allocation%20methods%208a38c0fe57a14a6694b1e82c2075060e/Untitled%202.png)

- 한 블록의 마지막 4바이트는 next block 의 번호를 가리키고 있다.
- next 블록을 따라가면서 파일을 읽는다.
- 불연속적으로 연결할 수 있고, 파일의 크기가 동적으로 커질 수 있다.
- file allocation block 에는 시작 블록의 넘버만 가지고 있다.

### Linked List 의 단점

- 파일 B를 읽기 위해 앞부분을 읽어야함
- 파일 데이터와 메타데이터를 하나에 가지고 있는건 좋은 방법이 아니다.
    - 안정성이 떨어진다.
    - 중간에 bad block이 있는경우 뒷 부분을 읽을 수 없다.
    - 높은 에러 발생률

### Linked List Allocation

![File%20allocation%20methods%208a38c0fe57a14a6694b1e82c2075060e/Untitled%203.png](File%20allocation%20methods%208a38c0fe57a14a6694b1e82c2075060e/Untitled%203.png)

- 모든 블록의 크기가 2^n 바이트가 아니면 여러번 읽게 된다.
- 2^n 바이트로 데이터를 저장하는 것이 좋다.

### Linked List Allocation Using a Table in Memory

![File%20allocation%20methods%208a38c0fe57a14a6694b1e82c2075060e/Untitled%204.png](File%20allocation%20methods%208a38c0fe57a14a6694b1e82c2075060e/Untitled%204.png)

- 모든 블록을 대표하는 포인터를 모아 메모리에 넣고 블록에는 순수하게 데이터만을 넣는다.
- 이것을 File Allocation Table이라 부르자.
- 7번 가보면 next는 2번이다. 2번 가보면 next는 10번이다.
- FAT가 이런식으로 이루어져있다.

### FAT

![File%20allocation%20methods%208a38c0fe57a14a6694b1e82c2075060e/Untitled%205.png](File%20allocation%20methods%208a38c0fe57a14a6694b1e82c2075060e/Untitled%205.png)

**Following File 1**

- 파일주소가 7번이고 사이즈가 3
    1. 7번 블록의 엔트리에 갔더니 8 => 다음 블록은 8
    2. 8번 블록의 엔트리에 갔더니 9 => 다음블록은 9
    3. 9번 블록의 엔트리에 갔더니 FF => 끝난거임

**Following Dir 1**

1. 10번 블록의 엔트리에 갔더니 FF => 끝난거임

- 디렉토리 블록은 가보니까 테이블이 있더라
- 유닉스는 이거랑 좀 다르다.

**단점?** 

- 큰 파일은 계속 쫓아 가야하는데 큰 파일의 경우 이를 쫓아가는게 오버헤드가 있다.
- 용량이 작은 경우 잘 동작하지만 용량이 크면 잘 동작하지 않는다.
- 최근의 큰 용량에서는 적절하지 않다.
- 호환성을 위해 지원을 하고 USB 같은 경우에 사용을 한다.

    => **NTFS** (indexed allocation)

## Allocated block management - Indexed allocation

![File%20allocation%20methods%208a38c0fe57a14a6694b1e82c2075060e/Untitled%206.png](File%20allocation%20methods%208a38c0fe57a14a6694b1e82c2075060e/Untitled%206.png)

- 불연속적으로 파일을 할당하는 방법 중 하나
- 괜찮은 방법이다.

- 인덱스 블록의 내용이 파일의 내용을 가리키는 포인터
- 큰 용량의 파일을 다루는게 FAT 보다 좋음
- 최근의 파일시스템이 이렇게 이루어짐
- 인덱스 블록은 하는 일에 비해 너무 큰 용량을 차지한다. => i-node block

### Design of UNIX File System

![File%20allocation%20methods%208a38c0fe57a14a6694b1e82c2075060e/Untitled%207.png](File%20allocation%20methods%208a38c0fe57a14a6694b1e82c2075060e/Untitled%207.png)

- 0,1 은 안쓰고 2번은 root dir의 i-node (그냥 안씀)
- i-node 번호만 알면 항상 i-node block을 찾을 수 있다.
- current working directory = 2 (when root dir is cur dir)
- Block size는 128 or 256 byte
- 데이터가 너무 크면 inode 가 모자랄 수 있다.
- 처음 inode의 개수 만큼만 파일을 만들 수 있었는데 최근에는 링크드 리스트처럼 더 확장할 수 있게 된다.

- inode에는 파일의 이름이 존재하지 않고, 디렉토리에 저장해둔다.
- 리눅스는 파일의 이름은 신경쓰지 않고 i-node 번호만을 신경쓴다.
- 파일은 하나인데 i-node는 여러개 일 수 있다.
- 파일이 삭제 되면 i-node 또한 free가 된다.
- 최초에는 DataBlock 13개를 작성할 수 있다.

**단점**

1. 안정성에 문제가 있다. super block 하나가 무너지면 전체에 접근할 수 없다
2. 구조적으로 성능이 나쁘다.
    - 하드디스크의 헤드의 움직임이 많아지기 때문에

이 두 문제를 버클리 대학에서 해결점을 제시함

=> Reducing Disk Arm Motion

### UNIX Block Addressing

![File%20allocation%20methods%208a38c0fe57a14a6694b1e82c2075060e/Untitled%208.png](File%20allocation%20methods%208a38c0fe57a14a6694b1e82c2075060e/Untitled%208.png)

- i node는 블록 13개 까지만 기록이 가능하다.
- 처음 10개는 직접 블록을 가리킨다.
- 10개 이후에는 트리구조로 블록을 가리킨다.
- 11 번째 인디렉트 블록
- 12 번째 더블 인디렉트 블록
- 13 번째 트리플 인디렉트 블록
- 파일의 크기가 작으면 바로 찾아갈 수 있고, 큰 파일을 쫓아가는데 생기는 오버헤드는 감수해야함
- 대부분의 파일은 작으니 직접 찾아갈 수 있음

### Implementing Directories (1)

![File%20allocation%20methods%208a38c0fe57a14a6694b1e82c2075060e/Untitled%209.png](File%20allocation%20methods%208a38c0fe57a14a6694b1e82c2075060e/Untitled%209.png)

- UNIX 에서 모든 파일은 이름과 i-node의 쌍을 이루고 있다.
- 하지만 파일 이름 길이에 한계가 있고 최대 크기를 잡아 놓으면 공간 낭비가 심하다.

⇒ 가변크기 디렉토리 엔트리

### Implementing Directories (2)

![File%20allocation%20methods%208a38c0fe57a14a6694b1e82c2075060e/Untitled%2010.png](File%20allocation%20methods%208a38c0fe57a14a6694b1e82c2075060e/Untitled%2010.png)

가변길이를 구현

- 파일이 삭제된다면 디렉토리 구조에서 또 다시 fragmentation 문제가 생긴다.
- 기존의 엔트리보다 작은 엔트리만 저장할 수 있다.
- 그래도 크게 문제 된다고 생각하지는 않음
- 그냥 새로 i-node 블록을 할당 받으면 된다.
- 파일 이름을 검색한다고 생각하면 linear search 로는 느릴 수 있어, 최신의 시스템은 트리 구조로 이를 저장해둔다.
- 파일의 이름에 대한 포인터만을 저장해 둘 수도 있다.
- fragmentation 문제가 발생하지 않는다.
- 그런데 불편한 면이 있어 사용하지 않는다.. ?

## Log-Structured File System

- Log-structured file system designed in Berkeley for UNIX to reduce disk seek times for the write operationsIn
- UNIX most of the write operations are small writes
- LFS considers the entire disk as a log and by buffering the writes in the memory, writes them in a single segment at the end of log periodically.
- Each segment may contain i-nodes, directory entry blocks and data blocks
- The problem is i-nodes are scattered all over the log instead of being in the fixed disk position
- 최근에 제안된 파일 시스템으로 write 성능이 좋은 것 같지만 utilization에 따라 오락가락한다.
- SSD 가 나오면서 각광받기 시작했다.

### LFS - File modification

![File%20allocation%20methods%208a38c0fe57a14a6694b1e82c2075060e/Untitled%2011.png](File%20allocation%20methods%208a38c0fe57a14a6694b1e82c2075060e/Untitled%2011.png)

- 파일이 수정되면 새로운 공간에, 새로운 i-node 를 갖는 파일을 만든다.
- 최신의 segment를 찾는것으로 파일 시스템을 최신으로 유지할 수 있다.
- inode map은 checkpoint 단위로 파일의 위치를 기억한다.
- 마지막 checkpoint 이후의 inode를 scan 함으로써 최신의 inode update를 할 수 있다.

=> 가능하면 모든 write 를 sequential 하게 하겠다.

![File%20allocation%20methods%208a38c0fe57a14a6694b1e82c2075060e/Untitled%2012.png](File%20allocation%20methods%208a38c0fe57a14a6694b1e82c2075060e/Untitled%2012.png)

- 파일을 업데이트 하게 되면, 파일을 업데이트, inode 수정, 빔맵 수정 => 디스크의 헤드가 여러번 왔다갔다 해야함
- log structure 의 경우 선형적으로 파일의 쓰기를 할 수 있다.

### LFS: Cleaning (Garbage Collection)

![File%20allocation%20methods%208a38c0fe57a14a6694b1e82c2075060e/Untitled%2013.png](File%20allocation%20methods%208a38c0fe57a14a6694b1e82c2075060e/Untitled%2013.png)

- Opening a file consists of using map to locate the i-node for that file
- LFS has a book keeping program named ***cleaner*** that moves around the log and remove old segments

- 새로운 파일이 저장되어 있다면 해당 로그는 invalid
- Garbage Collection의 효율이 중요하다.

## Journaling File Systems

- Operations required to remove a file in UNIX

### Journaling

- Logging before modifying file system structure
- Fast recovery from failure
- Example of Journaling File System
    - NTFS
    - Linux Ext3 FS
    - Reiser FS

운영체제의 파일 시스템은 일관성을 확인해야한다.

- 정상종료의 경우 OK
- 정상 종료의 기록이 없는 경우 파일 시스템의 정상 여부를 다시 확인해야 한다. 이 작업이 오래 걸릴 수 있다.
- logging 을 이용해 이를 빠르게 복구 할 수 있다.
- 우리가 알고 있는 대부분의 파일 시스템은 저널링을 사용한다.