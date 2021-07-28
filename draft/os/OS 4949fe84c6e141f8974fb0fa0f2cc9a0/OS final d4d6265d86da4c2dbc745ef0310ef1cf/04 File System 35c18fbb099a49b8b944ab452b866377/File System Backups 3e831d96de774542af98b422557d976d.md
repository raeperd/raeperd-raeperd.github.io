# File System Backups

- Backups to tape are generally made to handle one of two potential problems:
    - Recover from disaster.
    - Recover from stupidity.
- Backup Types
    - Physical Dump
    - Logical Dump
        - Complete Dump
        - Incremental Dump

![File%20System%20Backups%203e831d96de774542af98b422557d976d/Untitled.png](File%20System%20Backups%203e831d96de774542af98b422557d976d/Untitled.png)

- hole 이 있는 파일이 백업시에 큰 영향을 미친다.
- Directory 의 변경여부를 확인
- 파일을 덤프하기 위해서 상위의 디렉토리는 모두 덤프를 해야한다.

## Bitmaps used by the logical dumping algorithm

![File%20System%20Backups%203e831d96de774542af98b422557d976d/Untitled%201.png](File%20System%20Backups%203e831d96de774542af98b422557d976d/Untitled%201.png)

(a) 디렉토리는 모두 1, 파일은 변경된 것만 1

(b) 하위에 변경된 파일이 없는 디렉토리는 0으로 바꿈

(c) 디렉토리만 먼저 덤프를 한다.

(d) 파일을 모두 덤프한다.

- 같은 내용의 블록을 또 다시 덤프하지 않기위해 dduplication dump 라는게 있다.
- sparse matrix의 경우 효율적으로 저장할 수 있다.

## File system consistency

### Why consistency problems

- File & directory modification accompanies multiple block updates
- File system will be in inconsistent state if system crashes after some of those blocks are updated and the others are not.

### Utilities for recovering file system consistency

- UNIX: fsck
- Windows: scandisk

### Fast recovery of consistency

- Many modern file systems use journaling scheme that enables instant consistency recovery