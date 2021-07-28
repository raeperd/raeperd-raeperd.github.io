# File & File System

### File

- Basic unit for storing information in storage device
- Array of bytes
- Treated as a single entity by users
- Has unique pass nameAccess control

### File System

- Set of system software that provides services to users and applications in the use of files

- 파일은 단지 바이트의 연속일 뿐이다.
- 운영체제는 실행파일에 대해서만 파일의 구조를 약속한다.

### File Sharing Semantic

- 여러개의 프로세스가 하나의 파일에 접근하는 것이 가능하다.
- 유닉스에서는 새로운 데이터를 작성하는 즉시 볼 수 있다.
- 파일이 닫힌 이후에 업데이트 된 내용을 보여주는 것이 오버헤드를 줄인다.

# Directory

- Special file that contains information about files
    - Attribute
    - Location
    - Ownership
- Provide mapping between file names and the files themselves

## File Name and Path Name

- Absolute path
    - path from **root directory**
- Relative path
    - path from **current working directory**