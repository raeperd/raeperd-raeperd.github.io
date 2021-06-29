---
title: "Copy constructor and temporal obejcts"
date: 2019-02-25
draft: true
tags: ["c", "cpp"]
---

# 복사 생성자


## 생성자의 종류

1. 기본 생성자
2. 다중 정의된 생성자 (매개변수)

    2-1 변환 생성자 (매개변수가 1개) == [형 변환자.. ] 

3. 복사 생성자

    +) r-value 참조

4. 이동생성자 (Since C++11)

~> 이름이 없는, 코드에서 드러나지 않는 임시객체 때문에 여러 문제가 생긴다. 조심! 

 

## 함수 호출과 복사 생성자

 

    void TestFunc(CTest param) {...}

이 함수의 문제가 뭘까? 

해결방법

1. 복사 생성자를 삭제한다.
    - `CTest(const CTestData &rhs) = delete`
2. 매개변수의 형식을 참조자로 바꾼다. 

    void TestFunc(CTest &param) {...}

- 매개변수는 참조형이므로 새로운 객체를 생성하지 않는다!
- non-member 함수는 상수화 불가능하므로 매개변수를 상수화 함으로써 상수형 메소드 처럼 사용할 수 있다.

### Deep copy and Shallow copy

    #include <iostream>
    
    using namespace std;
    
    int main(void) {
        int* pa = new int(5);
        int* pb = nullptr;
    
        pb = pa;            // shallow copy
        pb = new int(*pa);  // deep copy
        
        return 0;
    }

![](Untitled-1930279b-d697-4bbf-9871-2928745f7ce6.png)

- Deep copy와 Shallow copy 에 대한 이해가 필요하다.
    - Deep copy : 원본을 여럿
    - Shallow copy : 원본을 가리키는 포인터를 여럿

### 복사 생성자 꼭 필요할까?

- 디폴트 복사 생성자가 하는 일은 Shallow copy다. Deep copy를 하려면 별도로 정의를 할 필요가 있다.
- 곧 클래스 내부에서 메모리를 동적 할당 및 해제하고, 이를 멤버 포인터 변수로 관리하고 있는 경우 복사 생성자는 따로 정의되어야만 한다.
- 반대로 멤버로 포인터를 가지고 있지 않다면 필요한 작업은 아니다.

## 대입 연산자

- 개발자가 정의한 객체도 대입 연산자가 먹힌다!
    - 모든 멤버 변수의 값을 복사함
- 그런데 Shallow copy로 복사한다.
    - 그래서 위에서처럼 포인터를 다룰 경우 다시 정의할 필요가 생긴다.

**정의하는 방법** 

    CTest& operator=(const CTest& rhs) {
    	*m_pnData = *rhs.m_pnData;
    	return *this;
    }

- `return *this`를 통해 a=b=c 와 같은 구문이 유효하다.

## 4.1.4 실습과제3

[실습과제 3: 복사 생성자와 대입 연산자](./3-bccd7807-695b-4830-a504-534df3212248.md)

# implicit type casting

---

## 변환 생성자

- 매개변수가 하나인 생성자를 변환 생성자라고도 하는데 이 친구가 은근슬쩍 호출되서 프로그램의 효율을 낭비하는 케이스가 생긴다.
- 불필요한 임시 객체를 만들어냄으로써 프로그램의 효율을 갉아먹는 원인이 된다,

    #include <iostream>
    #include <vector>
    
    using namespace std;
    
    class CTest {
    public:
        CTest() { cout << "CTest()" << endl;}
        CTest(int x) { // 변환 생성자 
            cout << "CTest(int)" << endl;
            CMyData = x; 
        }
        ~CTest() { cout << "~CTest()" << endl; }
    
        int GetData() const { return CMyData; }
        void SetData(int param) { CMyData = param;}
    
    private:
        int CMyData = 10;
    };
    
    void TestFunc(CTest param) {
        cout << "TestFunc()" << endl;
    }
    
    int main(void) {
        TestFunc(5);
        return 0;
    }

- main 함수의 `TestFunc(5)`는 에러를 낼 것 같지만 그렇지 않다.
- CTest 클래스는 int 자료형에 대한 변환 생성자를 제공하므로 함수의 매개변수 형식이 CTest라면 이것이 int가 될 수도 있다.
- 컴파일러가 5를 CTest의 인스턴스로 형변환을 해 임시객체를 만들어내기 때문

**결과**

> CTest(int)
TestFunc()
~CTest()

- 클래스 형식을 매개변수로 사용할때는.
    - 반드시 참조 형식을 사용한다
    - 묵시적 변환 생성자를 지원하는 클래스인지 확인해야 한다.

### **explicit**

    explicit CTest(int param) {
    	CMyData = param;
    }

- 이렇게 하면 사용자 코드에서 묵시적으로 변환이 일어나지 않게 한다.

## 허용되는 변환

- CTest 에서 int 자료형에 대한 변환 생성자를 제공한다면 int 자료형이 CTest형식으로 변환 될 수 있다.

    operator int(void) { return CMyData;}

- 반대로, CTest 자료형이 int 형으로 변하게 할 수도 있다.

그럼 아래와 같은 코드가 가능하다

    int main(void) {
        CTest test;
        int x(test);
    
    		cout << test << endl;
    		cout << (int)test << endl
        cout << static_cast<int>(test) << endl; 
    
    		return 0;
    }

- 형 변환 연산자에 `explicit`을 사용하면 `statc_cast<int>(test)` 처럼 형식을 명시해야만 형 변환이 가능해진다.
- 변환 생성자가 묵시적으로 호출되는 것을 `explicit`을 이용해 사용자 코드에서 보이지 않는 객체가 생성되고 소멸하는 것을 막을 수 있다.

# 임시 객체와 이동 시멘틱

---

- r-value가 임시객체일때 호출된다.

## 이름 없는 임시 객체

- 존재하는 인스턴스 이지만 '식별자'가 부여되지 않은 객체

    #include "pch.h"
    #include <iostream>
    
    using namespace std;
    
    class CTestData {
    private:
    	int m_nData = 0;
    	string m_pszName;
    
    public:
    	CTestData(const int nParam, const char* pszName) : m_nData(nParam), m_pszName(pszName) {
    		cout << __FUNCTION__ << "(int, char*)" << endl;
    	}
    
    	CTestData(const CTestData& rhs) : m_nData(rhs.m_nData) {
    		cout << __FUNCTION__ << "(const CTestData&)" << endl;
    	}
    
    	~CTestData() {
    		cout << __FUNCTION__ << endl;
    	}
    
    	CTestData& operator=(const CTestData &rhs) {
    		cout << __FUNCTION__ << endl;
    		this->m_nData = rhs.m_nData;
    		return *this;
    	}
    	int GetData() {
    		return this->m_nData;
    	}
    };
    
    CTestData TestFunc(int nParam) {
    	CTestData a(nParam, "a");
    	return a;
    }
    
    int main(int argc, char* argv[]) {
    	CTestData b(5, "b");
    	cout << "*****Before*****" << endl;
    	b = TestFunc(10);
    	cout << "*****After*****" << endl;
    	cout << b.GetData() << endl;
    	return 0;
    }

### 실행 결과

> CTestData(int, char*)
***Before***
CTestData(int, char*)
operator=
~CTestData
***After***
10
~CTestData

- Before 와 After 사이에 인스턴스가 하나 생성되었다가 삭제됨
- `b = TestFunc(10)` 의 코드가 생성되는 동안 이름없는 임시객체가 생성됬다가 소멸됨
    - 이름 없는 임시객체의 원본은 임시 객체의 복사 생성이 끝난 후 소멸한다.
- 코드에서는 들어나지 않는 이름 없는 임시객체가 생성된 후 소멸한다!

## r-value 참조

![](-5fc74ca6-1a13-438b-abbd-9807e49caabauntitled)

- 오버로드할 떄 호출시 모호성이 발생할 수 있다.

## 이동 시맨틱

- 복사 생성자와 대입 연산자에 r-value 참조를 조합해서 새로운 생성 및 대입의 경우를 만들어낸 것
- 어차피 사라질 객체를 깊은 복사를 하는 것이 아니라 얕은 복사를 수행해 성능을 높이는 것!

### 예제)

AFTER

    #include <iostream>
    
    using namespace std;
    
    class CTestData {
    private:
    	int m_nData = 0;
    	string m_pszName;
    
    public:
        CTestData() {
            cout << __FUNCTION__ << endl;
        }
    	CTestData(const int nParam, const char* pszName) : m_nData(nParam), m_pszName(pszName) {
    		cout << __FUNCTION__ << "(int, char*)" << endl;
    	}
    
    	CTestData(const CTestData& rhs) : m_nData(rhs.m_nData) {
    		cout << __FUNCTION__ << "(const CTestData&)" << endl;
    	}
        CTestData(const CTestData&& rhs) : m_nData(rhs.m_nData) {
            cout << __FUNCTION__ << "(const CTestData&& rhs)" << endl;
        }
    
    	~CTestData() {
    		cout << __FUNCTION__ << endl;
    	}
    
    	CTestData& operator=(const CTestData &rhs) {
    		cout << __FUNCTION__ << endl;
    		this->m_nData = rhs.m_nData;
    		return *this;
    	}
    	int GetData() const {
    		return this->m_nData;
    	}
        void SetData(int nParam) {
            this->m_nData = nParam;
        }
    };
    
    CTestData TestFunc(int nParam) {
        cout << __FUNCTION__ << " begin! " << endl;
    	CTestData a;
        a.SetData(10);
        cout << __FUNCTION__ << " done! " << endl;
    	return a;
    }
    
    int main(int argc, char* argv[]) {
        CTestData b;
    	cout << "*****Before*****" << endl;
        b = TestFunc(20);
        cout << "*****After*****" << endl;
        CTestData c(b);
        
    	return 0;
    }

### 실행 결과)

> CTestData
***Before***
TestFunc begin!
CTestData
TestFunc done!
operator=
~CTestData
***After***
CTestData(const CTestData&)
~CTestData
~CTestData