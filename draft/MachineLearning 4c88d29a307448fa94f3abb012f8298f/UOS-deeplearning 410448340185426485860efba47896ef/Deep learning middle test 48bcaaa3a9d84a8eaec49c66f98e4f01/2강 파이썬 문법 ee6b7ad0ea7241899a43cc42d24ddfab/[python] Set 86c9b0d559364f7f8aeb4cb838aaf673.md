# [python] Set

### 집합 생성

```python
# 집합 생성
s1 = set([1,2,3])
print(s1)

s2 = set("Hello UOS")
print(s2)
```

### 집합 연산

```python
# 집합연산
s1 = set([1, 2, 3, 4, 5, 6])
s2 = set([4, 5, 6, 7, 8, 9])

print(s1 & s2)  # 교집합
print(s1 | s2)  # 합집합
print(s1 - s2)  # 차집합
```

### 집합을 리스트나 튜플로 변환 가능

```python
# set을 리스트나 튜플로 변환 가능
s1 = set([1,2,3])
print(s1)

l1 = list(s1)
print(l1)

t1 = tuple(s1)
print(t1)

# 집합연산
s1 = set([1, 2, 3, 4, 5, 6])
s2 = set([4, 5, 6, 7, 8, 9])

print(s1 & s2)  # 교집합
print(s1 | s2)  # 합집합
print(s1 - s2)  # 차집합
```

### 집합 관련 함수

```python
# 원소 1개 추가
s1 = set([1, 2, 3])
s1.add(4)
print(s1)

# 여러 원소 추가
s1 = set([1, 2, 3])
s1.update([4, 5, 6])
print(s1)

# 원소제거
s1 = set([1, 2, 3])
s1.remove(2)
print(s1)
```