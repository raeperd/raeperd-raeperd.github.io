# [python] Dictionary

### 딕셔너리 요소 구성 및 추가

```python
a = {1: 'a'}
a[2] = 'b'
print("첫째문장수행결과: {0}".format(a) )

a['name'] = 'pey'
print("둘째문장수행결과: {0}".format(a) )

a[3] = [1,2,3]
print("셋째문장수행결과: {0}".format(a) )

# 삭제
del a[1]
print(a)
```

### 딕셔너리 관련 함수

```python
# Key 리스트 만들기(dict_keys라는 객체 생성)
a = {'name': 'pey', 'phone': '0119993323', 'birth': '1118'}
print(a.keys())

# Value 리스트 만들기(dict_values라는 객체 생성)
print(a.values())

# Key, Value 쌍 얻기(items)
# key와 value의 쌍을 튜플로 묶은 값을 dict_items 객체로 돌려준다.
print(a.items())

# Key로 Value얻기(get)
# a['name']과 결과가 동일하나 존재하지 않을 경우 get은 None을 반환하고, a['name']은 오류 발생
a = {'name':'pey', 'phone':'0119993323', 'birth': '1118'}
print(a.get('name'))
print(a['name'])

# 해당 Key가 딕셔너리 안에 있는지 조사하기(in)
print('name' in a)
print('email' in a)
```