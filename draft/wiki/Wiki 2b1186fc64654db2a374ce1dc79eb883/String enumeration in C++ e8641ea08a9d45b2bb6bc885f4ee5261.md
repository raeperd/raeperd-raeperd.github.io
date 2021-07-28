# String enumeration in C++

Category: cpp
Created: November 2, 2020 9:05 PM
Created By: RaeCheol Park
Last Edited Time: November 2, 2020 9:05 PM
Status: DONE

```cpp
#include "gtest/gtest.h"
#include <string>

TEST(StringTest, StringExpTest)
{
    std::string strContext = "a""b""c""d";
    EXPECT_EQ(strContext.size(), 4);
}
```

 이런 문법이 가능하다는 걸 이번에 알게됬다. 문자열로 된 파일을 읽어서 무언가를 하는 유닛테스트를 할때 보통 테스트용 파일을 만들고, 그걸 읽는 방식으로 구현했었는데, 아래와 같은 구현으로 긴 문자열을 소스코드에서 보이게 할 수 있다.

```cpp
#include "gtest/gtest.h"
#include <string>

TEST(StringTest, StringExpTest)
{
    std::string strContext = "DEVICE=enp0s31f6\n"
                             "TYPE=Ethernet\n"
                             "PROXY_METHOD=none\n"
                             "BROWSER_ONLY=no\n"
                             "BOOTPROTO=none\n"
                             "DEFROUTE=yes\n"
                             "IPV4_FAILURE_FATAL=no\n"
                             "IPV6INIT=yes\n"
                             "IPV6_AUTOCONF=yes\n"
                             "IPV6_DEFROUTE=yes\n"
                             "IPV6_FAILURE_FATAL=no\n"
                             "IPV6_ADDR_GEN_MODE=stable-privacy\n"
                             "NAME=enp0s31f6\n"
                             "UUID=d34f9fd2-a276-4ee9-8a5a-82b312754a37\n"
                             "ONBOOT=yes\n"
                             "IPADDR=192.168.1.64\n"
                             "PREFIX=24\n"
                             "GATEWAY=192.168.1.1\n"
                             "DNS1=8.8.8.8\n"
                             "IPV6_PRIVACY=no\n";
    EXPECT_EQ(strContext.substr(0,5), std::string("DEVICE"));
}
```

테스트 하나가 길어지지만 굳이 파일을 읽어보지 않아도 원본의 내용을 확인할 수 있다.

FILE IO 에 들어가는 리소스가 줄어드니까 성능상 이득도 볼 수 있다.