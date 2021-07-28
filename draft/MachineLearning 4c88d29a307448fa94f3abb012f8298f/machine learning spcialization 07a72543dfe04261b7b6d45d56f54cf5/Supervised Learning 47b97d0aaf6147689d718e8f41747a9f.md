# Supervised Learning

![Supervised%20Learning%2047b97d0aaf6147689d718e8f41747a9f/Untitled.png](Supervised%20Learning%2047b97d0aaf6147689d718e8f41747a9f/Untitled.png)

- yellow logistic, green is linear regression
    - cost function is diffrent

![Supervised%20Learning%2047b97d0aaf6147689d718e8f41747a9f/Untitled%201.png](Supervised%20Learning%2047b97d0aaf6147689d718e8f41747a9f/Untitled%201.png)

- linear regression 이 옳은지 확인하기 위해 loss function이 필요하다.

![Supervised%20Learning%2047b97d0aaf6147689d718e8f41747a9f/Untitled%202.png](Supervised%20Learning%2047b97d0aaf6147689d718e8f41747a9f/Untitled%202.png)

- Linear regression은 해를 가지지만 x^t*x 가 linearly independent 하다는 가정이 필요하지만 실제 데이터들은 그렇지 않을 수 있다.
- 역함수를 구하는데 필요한 복잡도는 무려 n cubic, 나이스한 알고리즘도 n^2.5 정도
- gradient descent 알고리즘은 이 보다 단순하고 값싼 연산으로 최저값을 구할 수 있다.

![Supervised%20Learning%2047b97d0aaf6147689d718e8f41747a9f/Untitled%203.png](Supervised%20Learning%2047b97d0aaf6147689d718e8f41747a9f/Untitled%203.png)

- activation function 이 diffrentiable 하다는 조건이 네트워크가 학습하기 어렵게 한다.
- Relu가 발견되고 나서 훨씬 빠른 최적화가 가능했다.
- 입력 층에서 다음 층으로 가는 weigth 매트릭스는 4 by 2
- linear activation function은 결과를 linear 하게 만들기 떄문에 큰 의미가 없음
- sigmoid 의 미분값에 0에 가까워짐에 따라 학습 속도가 느려진다.
- ELU는 relu의 미분값 0 문제를 해결하지만 계산적으로 비싸다

# Decision Tree

![Supervised%20Learning%2047b97d0aaf6147689d718e8f41747a9f/Untitled%204.png](Supervised%20Learning%2047b97d0aaf6147689d718e8f41747a9f/Untitled%204.png)

- Finding the optimal splitting when creating the trees is an NP hard problem there for greedy algorithm was used.
- In a decision classification tree, each decision of node consists of linear classifier of one feature

# Kernel method

![Supervised%20Learning%2047b97d0aaf6147689d718e8f41747a9f/Untitled%205.png](Supervised%20Learning%2047b97d0aaf6147689d718e8f41747a9f/Untitled%205.png)

- kenel method가 차원을 늘리는 것은 nn에서 weight 를 곱함으로써 차원을 늘리는 것과 비슷한 원리다.