# [python] class

# Class

```python
class Calculator:
    def __init__(self):
        self.result = 0

    def adder(self, num):
        self.result += num
        return self.result

		def __del__(self):
				self.result = 0
				print("distroyer called")

cal1 = Calculator()
cal2 = Calculator()

print(cal1.adder(3))
print(cal1.adder(4))
print(cal2.adder(3))
print(cal2.adder(7))
```

- self는 this pointer 와 같은 역할
- `__init__` : 생성자, `__del__` : 소멸자
- 인스턴스 변수는 `__init__` 에서 정의를 해야한다.