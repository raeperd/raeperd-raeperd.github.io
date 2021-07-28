# Coverage information from static library

Category: cpp
Created: November 2, 2020 9:02 PM
Created By: RaeCheol Park
Last Edited Time: November 2, 2020 9:02 PM
Status: DONE

정적 라이브러리의 단위테스트를 작성하다가 확인한 내용 

**--whole-archive**

For each archive mentioned on the command line after the **--whole-archive** option, include every object file in the archive in the link, rather than searching the archive for the required object files. This is normally used to turn an archive file into a shared library, forcing every object to be included in the resulting shared library. This option may be used more than once.Two notes when using this option from gcc: First, gcc doesn't know about this option, so you have to use **-Wl,-whole-archive**. Second, don't forget to use **-Wl,-no-whole-archive** after your list of archives, because gcc will add its own list of archives to your link and you may not want this flag to affect those as well.

from [https://linux.die.net/man/1/ld](https://linux.die.net/man/1/ld)

이 옵션을 주면 된다고 한다.

cmake에서 오류가 났던 걸로 기억하는데 사실은 되나보다 

[setup gcovr with cmake-How can I Get Code coverage recursively on all static libraries](https://stackoverflow.com/q/57819516/13662830)