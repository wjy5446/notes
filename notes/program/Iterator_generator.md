# Iterator and Generator

## Iterator

### 만들어진 원인

- 수많은 원소를 가지는 리스트를 저장할 경우, 많은 메모리가 소요
- 이를 해결하기 위해서, 현재 원소를 이용해 다음 값을 만들어 주는 `next`라는 함수가 있으면, 적은 메모리로 원소를 생성 가능



### Interator의 구조

- `__iter__()`을 가진 object
- `__iter__()`내부에 `__next__` method가 포함되어 있음



```
class Counter(object):
	def __iter__(self):
		iter = Iterator()
		return iter
		
class Iterator(object):
	def __init__(self):
		self.index = 0
		
	def __next__(self):
		if self.index > 10:
			raise StopIteration
		n = self.index * 2
		self.index += 1
		return n
```



### 정리

>- for loop에 객체에 `__iter__`가 있을 경우 iterator를 얻음
>- Iterator()의 `__next__`을 이용해 StopIteration이 나올 때까지 반복
>- `next()`을 이용해 하나씩 확인 가능



## Generator

iterator을 좀더 편하게 사용하는 방법



### Generator의 구조

```
def test():
	for i in range(10):
		yield i * 2
```

- yield가 포함
- 일반 함수와 차이점 :  호출시 generator 객체가 호출
- `next()`을 이용해서 실행된다, 이 때 yield가 표시된 부분까지만 실행, yield가 다 사용되면 StopIterator가 실행되고 종료.



## 예외처리

- 일반 함수 예외처리 처럼 동작
- 한번 에러 발생한 generator는 사용이 불가능
- finally 사용이 불가능하다.



## Coroutine

### 만들어진 이유

- generator의 경우, next()에서 호출되고 yield에서 발생한 값을 받는다. 즉, generator에서 값만 받을 수 있다.
- 반대로 generator에 값을 전달은 불가능하다. 이를 해결하기 위해 coroutine 만들어짐



### 구조

```
def coroutine1():
	print('callee 1')
	x = yield 1
	print('callee 2: %d' % x)

task = coroutine1()
i = next(task)
i = task.send(10)
```

- `send`을 이용해 값 전달