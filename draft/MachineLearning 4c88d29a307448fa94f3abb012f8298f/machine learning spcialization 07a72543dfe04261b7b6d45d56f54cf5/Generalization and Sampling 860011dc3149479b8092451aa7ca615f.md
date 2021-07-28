# Generalization and Sampling

# Generalization and ML models

- cost  function의 값이 0 이라고해서 항상 좋은 퍼포먼스를 보이는 것은 아니다.

Linear Regression의 Metric

- MSE
    - 큰 차이를 보이는 친구한테 더 큰 웨이트를 부여함
- RMSE
    - 실제 거리를 의미하니까 더 해석하기 좋음

## Overfitting

![Generalization%20and%20Sampling%20860011dc3149479b8092451aa7ca615f/Untitled.png](Generalization%20and%20Sampling%20860011dc3149479b8092451aa7ca615f/Untitled.png)

![Generalization%20and%20Sampling%20860011dc3149479b8092451aa7ca615f/Untitled%201.png](Generalization%20and%20Sampling%20860011dc3149479b8092451aa7ca615f/Untitled%201.png)

- 더 복잡한 모델을 사용하면 모든 특징을 잡아낼 수 있고 error 를 잡아낼 수 있다.
- 더 복잡한 모델은 더 많은 parameter 를 최적화 시켜야 한다.
- 데이터를 외우게 하는 것이 아니라 일반화를 시키려면 모르는 데이터에 대한 평가가 필요하다.

![Generalization%20and%20Sampling%20860011dc3149479b8092451aa7ca615f/Untitled%202.png](Generalization%20and%20Sampling%20860011dc3149479b8092451aa7ca615f/Untitled%202.png)

- 단순히 데이터를 분류하는 것만으로 오버피팅에 대한 정보를 확인할 수 있다.
- validation set 에 대한 성능이 좋지 않을때가 학습을 그만 해야할 때이다.

# When to Stop Model Training

![Generalization%20and%20Sampling%20860011dc3149479b8092451aa7ca615f/Untitled%203.png](Generalization%20and%20Sampling%20860011dc3149479b8092451aa7ca615f/Untitled%203.png)

- validation set에 대한 결과룰 보고 hyperparameter tuning 을 계속 할지 안할지를 결정한다.

![Generalization%20and%20Sampling%20860011dc3149479b8092451aa7ca615f/Untitled%204.png](Generalization%20and%20Sampling%20860011dc3149479b8092451aa7ca615f/Untitled%204.png)

- validataion set에 대한 검증만 통과했다고 해서 production이 가능한 것은 아니다.
- 언제 학습을 멈출지, 모델이 이미 validation set을 봐버렸기 때문

![Generalization%20and%20Sampling%20860011dc3149479b8092451aa7ca615f/Untitled%205.png](Generalization%20and%20Sampling%20860011dc3149479b8092451aa7ca615f/Untitled%205.png)

- test set 또한 필요하다. 단 한번의 테스트를 위해서!
- validataion은 통과하고 test를 통과하지 못한 경우에는 새로운 테스트를 할 수 없다.
    - 새로운 모델을 만들거나,
    - 모델을 위한 새로운 데이터를 더 모으거나
- test set은 낭비되고 있지 않느냐??

## Bootstrapping (Cross-Validataion)

![Generalization%20and%20Sampling%20860011dc3149479b8092451aa7ca615f/Untitled%206.png](Generalization%20and%20Sampling%20860011dc3149479b8092451aa7ca615f/Untitled%206.png)

- train validation split 을 여러번 하고 그 loss들의 평균을 사용한다.
- bootstraping or cross-validataion

데이터가 많다면, held-out test dataset 

데이가 별로 없다면 , cross-validataion

# Creating Repeatable Samples in BigQuery

```sql
#standardSQL
SELECT
 date,
 airline,
 departure_airport,
 departure_schedule,
 arrival_airport,
 arrival_delay
FROM
`bigquery-samples.airline_ontime_data.flights`

WHERE
 MOD(ABS(FARM_FINGERPRINT(date)),10) < 8
```

- 정렬을 하거나 하면 데이터에 편향이 생김
- 어떤 데이터가 training, test, validation인지 알필요가있음
- 해쉬 함수와 moduler 연산을 통해 이를 근사적으로 해결할 수 있다.

![Generalization%20and%20Sampling%20860011dc3149479b8092451aa7ca615f/Untitled%207.png](Generalization%20and%20Sampling%20860011dc3149479b8092451aa7ca615f/Untitled%207.png)

- ML project를 하기 위해서 작은 데이터에 대해 잘 작동하는 지를 먼저 확인하고 데이터의 양을 늘리는 것이 좋을 것이다.  (prototyping 처럼)

데이터를 나누는 방법

- hash some column
- modulo 70 == 0
- modulo 700 > 350
- modulo 700 > 525