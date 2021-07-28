# Disk Space Management

## Determining Block Size

### Large block size

- Waste space in case of small files
- Good for performance
- Small entries in FAT or block address list in i-node

### Small block size

- Good for space utilization
- Bad for performance
- Requires large number of entries in FAT or i-node

## Disk Space Management Block Size

![Disk%20Space%20Management%20783ace1d081d4fe587e227e4cf78ecbc/Untitled.png](Disk%20Space%20Management%20783ace1d081d4fe587e227e4cf78ecbc/Untitled.png)

- data rate of a disk and disk space efficiency
- 블록의 크기가 작으면 클러스터가 많아지고, 이들을 관리하는 오버헤드가 생긴다.
- 블록의 크기가 크면 전체 디스크 공간을 효율적으로 사용하지 못한다.

## Free space management

- Bit map
    - small, easy to find free blocks
- Linked List
    - Free blocks are chained by using pointer
    - Allocation & free overhead are big
- Grouping (Indexing)
    - Index block contain free block list
    - index blocks are chained

### Overhead of Bit map

- Bitmap requires extra space
- For example
    - Block size = 212 bytes
    - Disk size = 230 bytes (1 giga bytes)
    - N = 230/212 = 218 bits (or 32 Kbytes)
- Easy to get contiguos blocks

- 1GB를 32KB 로 관리할 수 있는건 장점이라 하자.

### Free space management - Linked List

![Disk%20Space%20Management%20783ace1d081d4fe587e227e4cf78ecbc/Untitled%201.png](Disk%20Space%20Management%20783ace1d081d4fe587e227e4cf78ecbc/Untitled%201.png)