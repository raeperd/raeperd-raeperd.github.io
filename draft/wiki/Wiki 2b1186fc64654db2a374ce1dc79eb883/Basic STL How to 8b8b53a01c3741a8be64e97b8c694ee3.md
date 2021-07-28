# Basic STL How to

Category: cpp
Created: November 2, 2020 9:03 PM
Created By: RaeCheol Park
Last Edited Time: November 2, 2020 9:03 PM
Status: DONE

# STL 자료구조, 알고리즘

SET / MULTISET 

- 트리 구조로 된 연결된 데이터
- value를 key처럼 사용

# vector

```cpp
#include <iostream>
#include <vector>

int main(void) {
    std::vector<int> vecTest; // 포인터만 있음
    vecTest.push_back(1);     // 4byte
    vecTest.push_back(2);     // 4*2byte
    vecTest.push_back(3);     // 4*4byte;
    vecTest.push_back(4);     // 4*4byte;
    vecTest.push_back(5);     // 8*4byte; 
    
    vecTest.resize(20);       // 확장을 해도 기존의 값까지 복사를 해준다. 
    vecTest[8]=8;
    vecTest[9]=9;

    size_t i;
    for(i=0;i<vecTest.size();i++)
        std::cout << vecTest[i] << std::endl;

    return 0;
}
```

- 메모리릭을 유발할 가능성이 1프로라도 있다면 쓰면 안된다.
- 

## vector tip

```cpp
#include <iostream>
#include <vector>

int main(void) {
    std::vector<unsigned char> momory;

    void* pBuffer = new char[1000];
    memset(pBuffer, 0, 1000);
    delete pBuffer; 

    return 0;
}
```

```cpp
#include <iostream>
#include <vector>

int main(void) {
    std::vector<unsigned char> memory;
    memory.resize(100);
    memset(&memory[0], 0, 100);

    return 0;
}
```

이렇게하면, 메모리를 헤제할 필요가 없다.  벡터가 알아서 해제해준다.

## list

```cpp
#include <iostream>
#include <list>

int main(void) {
    std::list<int> listTest;

    listTest.push_back(0);
    listTest.push_back(1);
    listTest.push_back(2);
    listTest.push_back(-1);
    listTest.push_back(-100);
    listTest.push_back(-2);
    listTest.push_front(100);
    listTest.insert(listTest.begin(), 9);
    
    std::list<int>::iterator iter;
    for(iter=listTest.begin(); iter!=listTest.end(); iter++)
        std::cout << *iter << std::endl;
    return 0;
}
```

## Stack

```cpp
#include <iostream>
#include <stack>

int main(void) {
    std::stack<int> stackTest;

    stackTest.push(1);
    stackTest.push(1);
    stackTest.push(1);
    stackTest.push(1);
    
    while(!stackTest.empty()) {
        std::cout << stackTest.top() << std::endl;
        stackTest.pop();
    }
    return 0;
}
```

- 큐나 스택은 클리어가 안된다.
    - 큐나 스택을 사용하려면 데이터를 엄격하게 관리해라는 STL 개발자의 암시? 가 아닐까

## map // multimap

```cpp
#include <iostream>
#include <map>
	
using namespace std;
	
int main(void) {
    multimap<int, const char*> mapTest;
    mapTest.insert(make_pair(0,"zero"));
    mapTest.insert(make_pair(0,"zero2"));
    mapTest.insert(make_pair(1,"one"));
	
	  map<int, const char*>::iterator iter;
	  for (iter = mapTest.begin(); iter!= mapTest.end(); iter++)
	      cout << iter->first << "-" << iter->second << endl;
		
		auto iter2 = maptest.find(2);
		if (iter2 != mapTest.end())
		{
			printf("%d", *iter);
	  }
	  return 0;
}
```

- 큰거부터 빼내기, 작은거 부터 빼내기할대는 map이나 set이 좋다.
- 중간을 접근해서 꺼낼 수 있다는게 장점이지만 왠만하면 `find` 이후에 써라
    - 없으면 만들어서 넣기 때문에 접근을 못해야하는 걸 접근할 수도 있다.

## Set // Multiset

1,4,16,64,

```cpp
#include <iostream>
#include <set>

using namespace std;

int main(void) {
    set<int> setTest;
    setTest.insert(4);
    setTest.insert(2);
    setTest.insert(10);
    setTest.insert(2);
    setTest.insert(10);
   
    for (auto iter = setTest.begin(); iter!= setTest.end(); iter++)
        cout << *iter << endl;

    for (auto iter = setTest.rbegin(); iter!= setTest.rend(); iter++)
        cout << *iter << endl; 
    
    return 0;
}
```

- `begin()` 과 `end()` 가 짝, `rbegin()`과 `rend()` 가 짝이다.
- `auto` 키워드로 깔끔하게 이터레이터를 할당할 수 있다.
- 중복제거할때 사용할 수 있음

# friend keyword

```cpp
class CMap {
	friend class CAccessor;
	int key;
	std::string value; 
}

class CAccesor {
	
}
```

반대

- open close 가 불분명하다.

모르는 것을 배우는 것보다 알게 모르게 가지고 있는 고정관념을 깨는 것이 중요하다.

어떤 container를 선택할지 유연하게 생각할 수 있어야 한다.