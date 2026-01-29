# 신경망 기초(2)

## 신경망 학습
- 신경망 학습
    - 신경망을 학습한다는 의미는 데이터를 보고 가중치를 수동으로 업데이트하는것이 아니라 자동으로 업데이트하는 것을 말한다
    - 신경망 학습에서는 최적의 매개변수(가중치와 편향)를 탐색 시 손실 함수(Loss Function)의 값을 가능한 한 작게 하는 매개변수 값을 찾는데, 이 때 손실함수의 기울기를 이용한다
    - 손실함수의 기울기를 이용하기 위해 매개변수의 미분(정확히는 기울기)을 계산하고, 이 미분값을 통해 매개변수의 값을 서서히 갱신하는 과정을 반복한다

## 훈련 데이터와 시험 데이터
- 훈련 데이터(Training Data)와 시험 데이터(Test Data)
    - 보통 기계 학습에서는 훈련 데이터와 시험 데이터로 학습과 실험을 진행한다.(좀 더 세부적으로는 검증 데이터도 있음)
    - 훈련 데이터와 시험 데이터로 나누는 이유는 모델에 대한 범용 능력을 평가하기 위함이다
    - 범용 능력이란 아직 보지 못한 데이터(즉, 훈련 데이터에 없는 데이터)로도 문제를 잘 풀어내는 능력을 의미한다
    - 먼저 훈련 데이터만 이용하여 학습하면서 최적의 가중치를 찾는다
    - 그 다음 시험 데이터를 이용하여 앞서 훈련한 모델의 성능을 평가한다

## 학습 데이터 vs 테스트 데이터
- Training MSE != test MSE
- 테스트 데이터에 대한 MSE를 줄이는 것이 중요

- Flexibility와 MSE
    - Flexible가 오르면 -> training MSE는 내려감
    - Flexible가 오르면 -> test MSE는 올라감
    - Overfitting 때문

## 손실 함수(Loss Function)와 종류
- 손실함수(Loss Function)
    - 우리는 신경망을 학습함으로써 실제값과 예측값의 오차인 손실을 최소화하여 실제값과 최대한 가깝게 예측하는 것이 목표
    - 예측값이 실제값에 가까우면 가까울수록 두 값의 차이가 적다는 뜻이므로 손실 함수 값이 작을수록 학습이 잘 되었다는 의미

- Regression 문제의 경우: MSE(Mean Squared Error) 평균제곱오차는 가장 많이 쓰이는 손실 함수로 예측값과 실제값의 차이를 제곱한 것을 평균으로 나눈 것이다
    - y는 예측값(신경망의 출력), t는 실제값(정답 레이블)
    - 예측값이 실제값에 가까우면 가까울수록 두 값의 차이가 적다는 뜻이므로 MSE값이 작을수록 학습이 잘 되었다는 의미

- Classification 문제의 경우: CEE(Cross Entropy Error)
    - 교차 엔트로피 오차(CEE)
    - y는 예측값(신경망의 출력), t는 실제값(정답 레이블)이다. 두 값 모두 확률값으로 0과1사이의 값
    - 정답의 출력이 전체 값을 정한다  
    => 예를 들어, one-hot 인코딩이 되어있는 경우에 정답 레이블이 '2'이고 '2'에 대한 신경망 출력이 0.6이라면, t는 one-hot 인코딩으로 인해 1이므로 CEE = −logₑ 0.6 = 0.51 이 된다  
    + one-hot 인코딩은 t에서 정답 클래스는 1, 나머지는 0으로 출력 -> 계산 단순, 확률적 해석 등의 이유  
    t가 0인 것들은 계산해도 0이므로 배제하고 t가 1인것들만 계산함

- 미니 배치(Mini-Batch) 학습
    - 훈련 데이터가 매우 많은 경우, 모든 데이터에 대한 손실 함수의 합을 구하려면 시간이 매우 오래 걸린다
    - 배치(batch) vs 미니 배치(mini-batch): 전체 데이터 vs 무작위 부분 데이터
        - 예를 들어, 60000개의 훈련 데이터 중에서 100개씩 차례로 학습하는 방법을 미니배치라고 한다
    - 배치 경사 하강법(Batch GD): 전체 데이터를 한 번에 사용
    - 확률 경사 하강법(Stochastic GD): 데이터를 한 개씩 처리하여 전체 데이터 반영. 여기서 확률의 의미는 전체 데이터를 한 번에 반영하지 않고 하나씩 반영하기 때문에 확률적으로 최종값을 찾아가는 과정을 거치게 된다는 의미
    - 미니 배치 확률 경사 하강법(Mini-batch Stochastic GD): 데이터를 미니 배치 단위로 처리하여 전체 데이터 반영

## 수치 미분
- 수치 미분
    - 미분의 정의는 특정 순간의 변화량
    - h를 한없이 0에 가깝게, 즉, 매우 작은 변화에 대한 f(x)의 변화량으로 이렇게 아주 작은 차분으로 미분하는 것을 수치 미분이라고 한다.
    - 이를 기계학습 관점에서 적용하면 가중치(w) 변화에 대한 손실 함수의 변화량이다
    - 밑에는 수치 미분 식을 코드로 구현한 코드이며, 변수 h는 무한히 0에 가깝게 하기 위해 매우 작은 값으로 구현했지만 여기에는 2가지 문제점이 있다
```python
def numerical_diff(f, x):
    h = 10e-50
    return (f(x+h) - f(x)) / h
```

- 문제점(1): 반올림 오차
    - 10e-50는 h를 0으로 무한히 가깝게 한 매우 작은 값을 의미한다
    - 하지만, 이를 10e-50을 출력해보면 0이 출력된다
    - 이렇게 너무 작은 값을 이용하면 컴퓨터로 계산하는데 문제가 발생한다
    - 따라서, 이용할 수 있는 적절한 값으로 1e-4정도를 이용한다
- 문제점(2): 차분 오차
    - 차분이란 임의의 두 점에서의 함수 값들의 차이를 말한다
    - 구현 코드를 보면, x+h와 x 사이의 함수f의 차분을 계산하고 있지만, 사실 이 계산에는 오차가 있다
    - x위치에서의 미분은 함수의 기울기(접선)에 해당하지만, 무분 식을 보면 x+h 와 x 사이의 기울기에 해당한다
    - 따라서, 이 두 식이 일치하려면 h가 무한히 0으로 가깝게 가야 하는데, 문제점(1)에서의 이유로 인해 차분 오차가 생길 수 밖에 없다
    - 이렇게 발생하는 오차를 줄이기 위해 중심 차분(중앙 차분)을 이용한다.
        - 최대 오차가 제한됨이 증명되어 있음
    - 중심 차분은 x를 중심으로 그 전후의 차분을 계산한다는 의미로 (x+h)와 (x-h)일 때의 함수 f의 차분을 계산한다
    - 아래 코드가 위 내용을 구현한 코드이며, 2*h는 (x+h) - (x-h)이다. 
    - 또한, h = 1e-4를 통해 반올림 오차 발생ㅇ르 줄이도록 했다
```python
def numerical_diff(f, x):
    h = 1e-4
    return (f(x+h) - f(x-h)) / (2*h)
```
- 이제 수치 미분이 잘 이루어지는지 다시 확인해보자.
    - 식은 𝑦 = 0.01𝑥2 + 0.1𝑥 이며, 𝑥가 5일 때와 10일 때를 비교해보겠다.
    - 직관적으로 계산해보면 𝑥가 5일 때의 미분결과는 0.2이고 𝑥가 10일 때의 미분결과는 0.3이다.
    - 코드 결과에서 알 수 있듯이 미분 결과와 거의 일치한다
```python
def function_1(x):
  return 0.01*x**2 + 0.1*x

print('x=5에서의 수치 미분 결과:', numerical_diff(function_1, 5))
print('x=5에서의 수치 미분 결과:', numerical_diff(function_1, 10))
-------------------------------------------------------------------
x=5에서의 수치 미분 결과: 0.1999999999990898
x=5에서의 수치 미분 결과: 0.2999999999986347
```

## 편미분
- 편미분
    - 지금까지는 같은 변수에 관한 미분에 대해서만 알아보았지만, 변수가 여러 개 존재하는 경우도 있다
    - 이 때 사용하는 것이 편미분이며, 편미분은 여러 변수 중에서 목표 번수에만 초점을 맞추고 다른 변수는 값을 고정(즉, 상수 취급)한다
    - 이해를 돕기 위해 예시를 들어보자
    - 문제: 아래의 식을 편미분하시오
        - y = f(x₀, x₁) = 3x₀⁴ + 2x₀²x₁² + 7x₁⁴
        - x₀에 대한 편미분일 경우 x₁을 상수 취급하고 미분을 진행하면 되고
            - ∂y/∂x₀ = 12x₀³ + 4x₀x₁²
        - x₁에 대한 편미분일 경우 x₀을 상수 취급하고 미분을 진행하면 된다다
            - ∂y/∂x₁ = 4x₀²x₁ + 28x₁³
    
    - 이제 이를 바탕으로 특정 지점에서의 편미분을 구해보겠다
    - 식 3x₀⁴ + 2x₀²x₁² + 7x₁⁴이 주어졌고, x₀ = 3, x₁ = 4일 때 x₀에 대한 편미분을 구하는 문제가 있다고 하자
    - 이전 슬라이드에서 구한 답을 통해 직관적으로 답을 구해보면, 12 * 3^3 + 4 * 3 * 4^2 = 516이다
    - 코드 결과에서 알 수 있듯이 미분 결과와 거의 일치한다
```python
def partial_diff(f, x):
    h = 1e-4  # 0.0001
    return (f(x + h) - f(x - h)) / (2 * h)

def function_1(x0):
    x1 = 4
    return 3 * (x0**4) + 2 * (x0**2) * (x1**2) + 7 * (x1**4)

print('x0=3, x1=4에서의 x0에 대한 편미분 결과 :', partial_diff(function_1, 3))
-----------------------------------------------------------------------------
x0=3, x1=4에서의 x0에 대한 편미분 결과 : 516.0000003616005
```

- 이번엔 x₁에 대한 편미분
```python
def partial_diff(f, x):
    h = 1e-4  # 0.0001
    return (f(x + h) - f(x - h)) / (2 * h)

def function_1(x1):
    x0 = 3
    return 3 * (x0**4) + 2 * (x0**2) * (x1**2) + 7 * (x1**4)

print('x0=3, x1=4에서의 x0에 대한 편미분 결과 :', partial_diff(function_1, 4))
----------------------------------------------------------------------------
x0=3, x1=4에서의 x0에 대한 편미분 결과 : 1936.0000011192824
```

## 손실함수의 기울기
- 손실 함수의 기울기
    - 우리는 손실 함수의 값을 가능한 한 작게 하는 매개변수(가중치와 편향)를 찾는 것이 목표이며, 이를 위해 손실함수의 기울기를 이용한다
    - 손실함수의 기울기를 이용하기 위해 매개변수의 미분(정확히는 기울기)을 계산하고, 이 미분값을 통해 매개변수의 값을 서서히 갱신하는 과정을 반복한다
    - 변수가 여러 개인 경우에 대한 미분도 다뤘으나 각 변수마다 따로 구했으므로 이를 묶어서 동시에 계산이 되도록 한다
    - 이렇게 x₀와 x₁을 동시에 넣어주면 그에 대한 결과가 동시에 출력된다
```python
import numpy as np
def numerical_gradient(f, x):
  h = 1e-4
  grad = np.zeros_like(x) # x와 형상이 같은 배열을 생성

  for idx in range(x.size):
    tmp_val = x[idx]
    # f(x+h) 계산
    x[idx] = tmp_val + h
    fxh1 = f(x)

    # f(x-h) 계산
    x[idx] = tmp_val - h
    fxh2 = f(x)

    grad[idx] = (fxh1 - fxh2) / (2*h)
    x[idx] = tmp_val # 값 복원

  return grad

def function_2(x):
  return 3*(x[0]**4) + 2*(x[0]**2)*(x[1]**2) + 7*(x[1]**4)

print('x0=3, x1=4에서의 x0, x1에 대한 편미분 결과:', numerical_gradient(function_2, np.array([3.0, 4.0])))
--------------------------------------------------------------------------------------------
x0=3, x1=4에서의 x0, x1에 대한 편미분 결과: [ 516.00000036 1936.00000112]
```

## 경사하강법(Gradient Descent)
- 경사하강법
    - 우리는 예측값과 실제값을 바탕으로 손실함수가 가장 최소일 때의 가중치(w)와 편향(b)을 찾아야 하며, 이를 위해 경사하강법을 이용한다
    - 경사하강법(Gradient Descent)이란 손실 함수에 대한 경사(기울기)의 절대값을 줄여가면서 손실함수를 최소화하는 가중치와 편향을 찾는 알고리즘이다

- 학습률(Learning rate)
    - 학습률은 모델이 한 번 학습할 때 얼마만큼 학습해야 하는지에 대한 정도를 나타내며, 모델이 학습하면서 오차를 줄이기 위해 경사하강법을 적용할 시 얼마만큼 경사각을 내려갈 것인지에 대한 지표이기도 하다
    - 학습률에 따라 1번의 학습을 한 이후에 가중치가 갱신된다
    - 아래 예제: (x0, x1) = (0, 0)일 때 함수 f2가 최소값 0을 가짐
    - Gradient가 음수이면 오른쪽, 양수이면 왼쪽으로 가야되기 때문에 W = W - learning rate * gradient
```python
import matplotlib.pylab as plt

def gradient_descent(f, init_w, learning_rate, step_num):
  w = init_w

  for i in range(step_num):
    grad = numerical_gradient(f, w)
    w -= learning_rate * grad

  return w

def function_2(x):
  return 3*(x[0]**4) + 2*(x[0]**2)*(x[1]**2) + 7*(x[1]**4)

print('Learning rate가 10.0일때 경사하강법을 적용한 W 위치:', 
      gradient_descent(function_2, np.array([-3.0, 4.0]), 10.0, 100))
print('Learning rate가 0.001일때 경사하강법을 적용한 W 위치:', 
      gradient_descent(function_2, np.array([-3.0, 4.0]), 0.001, 100))
print('Learning rate가 0.00000001일때 경사하강법을 적용한 W 위치:', 
      gradient_descent(function_2, np.array([-3.0, 4.0]), 0.00000001, 100))
-----------------------------------------------------------------------------
Learning rate가 10.0일때 경사하강법을 적용한 W 위치: [-9.37416128e+13  2.05109945e+15]
Learning rate가 0.001일때 경사하강법을 적용한 W 위치: [-0.58538933  0.35407636]
Learning rate가 0.00000001일때 경사하강법을 적용한 W 위치: [-2.99948419  3.99806535]
```

- 학습률(Learning rate)
    - 학습률에 따라 겅사하강법은 좋은 결과를 얻을 수도 있고 나쁜 결과를 얻을 수도 있기 때문에 적절한 학습률을 설정하는 것이 매우 중요하다
    - 1번째 경우에는 (-3, 4)에서 숫자가 매우 커진 것을 보아 발산한 것을 알 수 있다
    - 2번째 경우에는 (-3, 4)에서 거의 (0, 0)으로 이동한 것으로 보아 가장 좋은 결과를 얻었다
    - 3번째 경우에는 (-3, 4)에서 거의 갱신이 이루어지지 않았다

    - 모델 학습 시 학습률이 너무 크면 Loss function의 최저점으로 가지 못하고 발산할 수가 있으며, 학습률이 너무 작으면 학습시간이 매우 오래 걸릴뿐만 아니라 최저점에 도달하지 못할 수가 있다
    - 따라서, 모델 학습 시 적절한 학습률을 설정해야 한다

## 오차 역전파
- 오차 역전파
    - 지금까지 신경망 학습을 윟 ㅐ신경망의 매개변수(가중치, 편향)에 대한 손실 함수의 기울기를 위해 수치 미분을 사용했다
    - 수치 미분은 단순하고 구현하기도 쉽지만 계산이 오래 걸린다는 단점이 있다
    - 따라서, 매개변수에 대한 손실 함수의 기울기를 효율적으로 계산하는 오차 역전파(또는 역전파)에 대해 알아본다
    - 하지만, 역전파를 수식으로 바로 접하면 어려울 수 있으니 계산 그래프와 간단한 예제부터 알아본다

- 계산 그래프
    - 계산 과정을 그래프로 나타낸 것
    - 계산그래프는 계산 과정을 노드와 화살표로 표현하며, 노드는 원으로 표기하고, 원 안에 연산 내용을 넣는다
    - 문제1: 영희가 슈퍼에서 1개의 100원인 사과를 2개 샀다. 소비세 10%가 부과된다. 지불 금액을 구하라.
    - 문제1의 계산 그래프는 아래와 같이 표현할 수 있다
    - 왼쪽에서 오른쪽으로 진행하는 것을 순전파라고 한다

    - 문제2: 영희는 슈퍼에서 사과를 2개, 귤을 3개 샀다. 사과는 100원, 귤은 150원이다. 소비세가 10%일때 지불 금액을 구하라.

- 국소적 계산
    - 계산 그래프의 특징은 '국소적 계산'을 전파함으로써 최종 결과를 얻는다는 점이다
    - '국소적'은 '자신과 직접 관계된 작은 범위'이라는 뜻으로 전체에서 어떤 일이 벌어지든 상관없이 자신과 관계된 정보만으로 다음 결과를 출력할 수 있다.
    - 이는 전체가 아무리 복잡해도 각 노드는 자신과 관계된 노드만 집중하면 되기 때문에 문제를 단순화할 수 있다는 것을 의미한다

    - 여러 식품을 구입하여(즉, 복잡한 계산) 총 금액이 4000원이 되었다
    - 사과와 구입한 여러 식품 값을 더하는 계산은 4000원이라는 숫자가 어떻게 계산되었는지에 단순하게 두 숫자를 더하면 된다는 점에서 국소적 계산이 적용된 것이다

- 오차 역전파
    - y=f(x)에서 y값을 변경시키려면 x수식을 어떻게 변경해야 할까?의 문제
    - 아까 문제1에서 왼쪽에서 오른쪽으로 진행하는 순전파만 표시했다면 아래 그림은 오른쪽에서 왼쪽으로 진행하는 역전파도 표시했다
    - 역전파의 결과로부터 '사과 가격에 대한 지불 금액의 미분'값은 2.2이며, 이는 사과가 1원 오르면 최종 금액은 2.2원 오른다는 의미이다  
    => 좀 더 정확히는, 사과 값이 아주 조금 오르면 최종 금액은 아주 조금 오른 값의 2.2배만큼 오른다는 의미이다

    - 최종 결과(사과 가격에 대한 지불 금액의 미분) 말고도 '소비세에 대한 지불 금액의 미분'이나 '사과 개수에 대한 지불 금액의 미분'도 구할 수 있다
    - 이렇게 역전파는 '국소적인 미분'을 오른쪽에서 왼쪽으로 연쇄법칙을 통해 전달한다

    - 계산 그래프에서의 역전파에 대해 좀 더 자세히 알아보도록 하자
        - y=f(x)의 역전파로 신호 E에 노드의 국소적 미분(dy/dx)을 곱한 후 다음 노드로 전달한다
        - 국소적 미분은 순전파 때의 y = f(x)의 미분을 구한다는 의미이다
        - 이러한 방식을 이용하면 목표로 하는 미분 값을 효율적으로 구할 수 있는데, 이 때 사용되는 것이 연쇄법칙이다

- 연쇄 법칙
    - 연쇄 법칙은 합성 합수의 미분에 대한 성질로 합성 함수의 미분은 합성 함수를 구성하는 각 함수의 미분의 곱으로 나타낼 수 있다
    - 예를 들어, z = (t)**2 이라는 식이 있고, t = x + y인 경우  
    - z = f(t), t = g(x) -> z = f(g(x))
    - 아래의 원리에 의해 dz/dx = 2(x+y)로 나타낼 수 있다
    - dz/dx = (dz/dt) * (dt/dx) = 2t * 1 = 2(x+y)

- 연쇄 법칙을 이용한 오차 역전파
    - 사과 가격을 a(=100), 사과 개수를 b(=2), 소비세를 c(=1.1), 최종결과를 z(=200)라 하자
    - z = a * b * c 이며 dz/da = bc = 2.2, dz/db = ac = 110, dz/dc = ab = 200 이다

    - 소비세의 미분은 dz/dc = 200, 사과 개수의 미분은 dz/db = ac = 110, 사과 가격의 미분은 dz/da = bc = 2.2 이다.
    - 최종 결과의 미분은 dz/dz = 1이고, (사과 가격 * 사과 개수)의 미분은 dz/d(ab) = c = 1.1 이다.

- 활성화 함수 계층 학습
    - 우리는 이전 시간에 MNIST에 대한 순전파를 배웠다.
    - 이제 역전파를 계산해야 하는데, 원리는 똑같지만 활성화 함수의 경우 식으로 인해 수식이 조금 복잡하므로 따로 다룬다

## 활성화 함수 계층 학습
- sigmoid 순전파
    - sigmoid 식의 순전파. 아래 식을 x부터 순차적으로 진행해서 만듦
    - y = 1 / (1 + exp(-x))

- sigmoid 역전파
    - 아래 그림은 sigmoid의 역전파의 최종 결과를 나타낸 것이다
    - sigmoid의 역전파의 최종 결과가 왜 이렇게 나오는지에 대해 알아보자
        - 이번 강의에서는 sigmoid의 역전파에 대해서만 다룬다
        - 나머지 활성화 함수들도 원리는 같다  
    x ──▶ (sigmoid) ──▶ y  
∂L/∂y ──▶ (sigmoid) ──▶ ∂L/∂y · y(1 − y)  
+ 결국 출력에서 입력 방향으로 국소 미분을 이용해 한 단계씩 미분값을 곱해가며 거꾸로 이동하는 과정


- 출력층 (softmax / loss function) 학습
    - pdf에 있는 그림을 보면 손실함수 계층이 원래는 존재한다는것을 알 수 있다
    - 이 그림에서는 손실함수 Cross Entropy Error를 사용한다

## Tensorflow/Keras 활용하기
- Tensorflow/Keras 
    - Tensorflow는 구글에서 개발하고 오픈소스로 공개한 머신러닝 프레임워크이다
    - Tensorflow는 처음 머신러닝을 접하는 사람이 사용하기엔 어려운 부분이 많다
    - Keras는 Tensorflow 위에서 동작하는 프레임워크로 사용자가 이용하기 쉽게 개발되었다
    - Tensorflow로 신경망을 만드는 경우 직접 선언해줘야 하는 부분이 많지만 Keras의 경우 몇 줄만으로 간단하게 만들 수 있다

    - Tensorflow는 Tensor가 흐른다는 뜻으로, Tensor를 포함한 계산을 정의하고 실행하는 프레임워크이다
    - Tensor는 벡터와 행렬을 일반화한 것으로 Tensorflow가 연산하기 위해 사용하는 자료형, 데이터의 형태, 고차원으로 확장 가능하다
    - 쉽게 말해, 우리가 이전에 다뤘던 다차원의 numpy array와 비슷한 개념이라고 생각하면 된다

- Tensorflow로 모델 구현(LSTM)
    - Tensorflow는 처음 머신러닝을 접하는 사람이 사용하기엔 어려운 부분이 많다
    - 하지만, 직접 구현하는 만큼 원하는 부분에 대한 확장이 용이하다

- Keras로 모델 구현(LSTM)
    - Keras는 Tensorflow 위에서 동작하며, 사용자가 이용하기 쉽게 개발되었다
    - 이전 슬라이드에서와 다르게 코드 몇 줄만으로 동일한 모델을 이용할 수 있다
    - 구현되어 있는 라이브러리를 이용하는 만큼 모델 자체를 확장하는 것은 쉽지 않다
    - 따라서, 뒤의 실습 역시 keras를 이용한다

# 신경망 실습(1)

## 손실 함수(Loss Function)
- MSE(Mean Squared Error)
    - 평균제곱오차(MSE)는 가장 많이 쓰는 손실함수로 예측값과 실제값의 차이를 제곱한 것을 평균으로 나눈 것이다
```python
import numpy as np

def mean_squared_error(y, t):
  return np.sum((y-t)**2)/len(y)

t = [0, 0, 1, 0, 0, 0, 0, 0, 0, 0]
y1 = [0.1, 0.0, 0.6, 0.0, 0.05, 0.1, 0.05, 0.1, 0.0, 0.0]
y2 = [0.1, 0.1, 0.05, 0.0, 0.05, 0.1, 0.0, 0.6, 0.0, 0.0]

print('예측값이 y1일 때 MSE:', round(mean_squared_error(np.array(y1), np.array(t)), 4))
print('예측값이 y2일 때 MSE:', round(mean_squared_error(np.array(y2), np.array(t)), 4))
--------------------------------------------------------------------------------------
예측값이 y1일 때 MSE: 0.0195
예측값이 y2일 때 MSE: 0.1295
```

- CEE(Cross Entropy Error)
    - np.log()에 0을 입력하면 마이너스 무한대가 되므로 이를 해결하기 위해 아주 작은 값인 1e-7을 더한다
```python
def cross_entropy_error(y, t):
  delta = 1e-7
  return -np.sum(t * np.log(y + delta))

t = [0, 0, 1, 0, 0, 0, 0, 0, 0, 0]
y1 = [0.1, 0.0, 0.6, 0.0, 0.05, 0.1, 0.05, 0.1, 0.0, 0.0]
y2 = [0.1, 0.1, 0.05, 0.0, 0.05, 0.1, 0.0, 0.6, 0.0, 0.0]

print('예측값이 y1일 때 CEE:', round(cross_entropy_error(np.array(y1), np.array(t)), 4))
print('예측값이 y2일 때 CEE:', round(cross_entropy_error(np.array(y2), np.array(t)), 4))
----------------------------------------------------------------------------------------
예측값이 y1일 때 CEE: 0.5108
예측값이 y2일 때 CEE: 2.9957
```

- CEE(Cross Entropy Error) - 미니 배치용
    - 아래 코드 역시 CEE지만 미니 배치 데이터를 처리할 수 있는 CEE이다
    - y가 1차원이라면, 즉, 데이터 1개당 CEE를 구하는 경우라면 reshape 함수로 데이터의 형상을 바꿔주고 y가 1차원이 아니면 그대로 둔다
    - 그 다음 미니 배치의 크기로 나누어서 데이터 1개당 평균의 CEE를 계산한다
```python
def cross_entropy_error(y, t):
  if y.ndim == 1:
    t = t.reshape(1, t.size)
    y = y.reshape(1, y.size)

  batch_size = y.shape[0]
  return -np.sum(t*np.log(y)) / batch_size
```

## 미니 배치(Mini-Batch)
- 미니 배치
    - 아래 코드는 미니 배치만큼 학습하기 위해 60000개의 훈련 데이터 중에서 10개의 데이터를 무작위로 추출한다

```python
import numpy as np
from dataset.mnist import load_mnist    # dataset.mnist 파일이 없어서 실습 불가능

# MNIST 데이터 불러오기
(x_train, t_train), (x_test, t_test) = load_mnist(
    normalize=True,                 # 픽셀값을 0~255 -> 0~1로 바꿈(정규화)
    one_hot_label=True              # 정답을 one-hot 벡터로 변환/즉,정답인것만 1 나머지는 0
)

print('x_train 형상:', x_train.shape)   # x_train 형상: (60000, 784) 이미지 한 장은784
print('t_train 형상:', t_train.shape)   # t_train 형상: (60000, 10) 정답은 클래스 10개

# 학습 데이터 개수
train_size = x_train.shape[0]

# 미니 배치 크기
batch_size = 10       # 한 번 학습할 때 10개 데이터만 사용 -> 전체 6만개를 한 번에 쓰지 않음

# 랜덤하게 batch_size만큼 인덱스 선택
batch_mask = np.random.choice(train_size, batch_size)

# 미니 배치 데이터 추출
x_batch = x_train[batch_mask]
t_batch = t_train[batch_mask]

print('train data 크기에서 Mini-Batch 수만큼 랜덤하게 추출한 값:')
print(batch_mask, sep='\n')

print('Mini-Batch 처리된 x_train:')
print(x_batch, sep='\n')

print('Mini-Batch 처리된 t_train:')
print(t_batch, sep='\n')
```

## 수치 미분
- 수치 미분
    - 위에서 함

## 편미분
- 편미분
    - 위에서 함

## 손실함수의 기울기
- 손실함수의 기울기
    - 위에서 함

## 경사하강법
- 경사하강법
    - 위에서 함

## 활성화 함수 계층
- Sigmoid 계층
    - 아래 코드는 sigmoid의 순전파(forward)와 역전파(backward)를 구현한 것이다
    - 순전파의 출력을 인스턴스 변수 out에 저장했다가, 역전파 실행 시 저장했던 변수를 사용하여 계산한다. dout=dL/dy, out=y
```python
def sigmoid(x):
    return 1 / (1 + np.exp(-x))


class Sigmoid:
    def __init__(self):
        self.out = None

    def forward(self, x):
        out = sigmoid(x)
        self.out = out
        return out

    def backward(self, dout):
        dx = dout * (1.0 - self.out) * self.out
        return dx
```

- 출력층( Softmax/손실함수(CEE))
    - 순전파에서는 softmax를 거쳐 손실함수인 CEE를 출력한다
    - 역전파에서는 전파하는 값을 미니 배치 수로 나눠서 데이터 1개 당 오차를 앞의 계층으로 전파한다
```python
class SoftmaxWithLoss:
    def __init__(self):
        self.loss = None   # 손실
        self.y = None      # softmax의 출력
        self.t = None      # 정답 레이블

    def forward(self, x, t):
        self.t = t
        self.y = softmax(x)
        self.loss = cross_entropy_error(self.y, self.t)
        return self.loss

    def backward(self, dout=1):
        batch_size = self.t.shape[0]
        dx = (self.y - self.t) / batch_size
        return dx
```

- 출력층( Softmax/손실함수(CEE))
    - 순전파에서 사용되는 softmax와 손실함수인 CEE가 있는데, 이전 시간에 배웠던 코드와 약간 다르다
    - softmax의 경우 2차원 데이터를 다루기 위해 바뀌었고 기계 학습 시 대부분 미니 배치로 학습을 하므로 미니 배치용 CEE를 사용한다
```python
# 2차원 데이터 처리를 위한 softmax
def softmax(x):
    x = x - np.max(x, axis=-1, keepdims=True)
    return np.exp(x) / np.sum(np.exp(x), axis=-1, keepdims=True)

# 미니 배치용 CEE
def cross_entropy_error(y, t):
    if y.ndim == 1:
        t = t.reshape(1, t.size)
        y = y.reshape(1, y.size)

    batch_size = y.shape[0]
    return -np.sum(t * np.log(y)) / batch_size
```

## MNIST 학습
- MNIST
    - MNIST는 손글씨로 작성한 이미지로부터 어떤 숫자인지 분류하는 예제
    - 이전 강의에서서 실습했던 구조를 그대로 구현하며, 이전 강의에서는 학습이 되어있는 신경망에서 순잔파만 진행했다면 본 강의에서는 처음 상태에서 순전파/역전파를 이용해 학습해본다

- Affine 계층
    - 신경망의 순전파 시 수행하는 행렬의 내적을 수행하는 계층으로 모든 layer에 포함되어 있다고 생각하면 된다
```python
class Affine:
    def __init__(self, W, b):
        self.W = W
        self.b = b

        self.x = None
        self.original_x_shape = None
        self.dW = None
        self.db = None

    def forward(self, x):
        self.original_x_shape = x.shape
        x = x.reshape(x.shape[0], -1)
        self.x = x

        out = np.dot(self.x, self.W) + self.b
        return out

    def backward(self, dout):
        dx = np.dot(dout, self.W.T)
        self.dW = np.dot(self.x.T, dout)
        self.db = np.sum(dout, axis=0)

        dx = dx.reshape(*self.original_x_shape)
        return dx
```

- 신경망 구현
    - params는 dictionary 변수로 신경망의 매개변수(가중치, 편향)를 저장한다
    - layers는 신경망의 계층을 보관하고 Affine과 Sigmoid계층을 순서대로 유지하며, lastLater에는 신경망의 마지막 계층인 SoftmaxWithLoss 계층이 있다
```python
class ThreeLayerNet:
    def __init__(self, input_size, hidden_size1, hidden_size2, output_size,
                 weight_init_std=0.01):
        # 가중치 초기화
        self.params = {}
        self.params['W1'] = weight_init_std * np.random.randn(input_size, hidden_size1)
        self.params['b1'] = np.zeros(hidden_size1)
        self.params['W2'] = weight_init_std * np.random.randn(hidden_size1, hidden_size2)
        self.params['b2'] = np.zeros(hidden_size2)
        self.params['W3'] = weight_init_std * np.random.randn(hidden_size2, output_size)
        self.params['b3'] = np.zeros(output_size)

        # 계층 생성
        self.layers = OrderedDict()
        self.layers['Affine1'] = Affine(self.params['W1'], self.params['b1'])
        self.layers['Sigmoid1'] = Sigmoid()
        self.layers['Affine2'] = Affine(self.params['W2'], self.params['b2'])
        self.layers['Sigmoid2'] = Sigmoid()
        self.layers['Affine3'] = Affine(self.params['W3'], self.params['b3'])

        self.lastLayer = SoftmaxWithLoss()
```

- (앞에 ThreeLayerNet class에 이어지는 코드)
    - predict 함수는 구성된 각 계층에 대해 순전파를 진행하여 예측값을 구하는 함수이다
    - self.layers.values()에는 구성한 layer들이 들어가 있으며, for문을 돌면서 마지막 계층까지 순전파를 진행하고 그에 대한 결과(예측값)를 return 한다
    - accuracy 함수도 이전 슬라이드의 ThreeLayerNet Class의 포함되게 입력해야 한다
```python
def predict(self, x):
    for layer in self.layers.values():
        x = layer.forward(x)
    return x
```

- (앞에 ThreeLayerNet class에 이어지는 코드)
    - loss 함수는 말 그대로 손실함수의 값을 구하는 함수로 predict 함수를 통해 예측한 값과 t인 정답 값을 lastLayer인 SoftmaxWithLoss 계층으로 보낸 후의 결과를 return 한다
```python
# x : 입력 데이터, t : 정답 레이블
def loss(self, x, t):
    y = self.predict(x)
    return self.lastLayer.forward(y, t)
```

- (앞에 ThreeLayerNet class에 이어지는 코드)
    - accuracy 함수는 말 그대로 정확도를 구하는 함수이다
    - 먼저 predict 함수를 통해 예측한 10개의 결과(0~9일 확률)를 y에 저장한다
    - 그 다음 예측한 10개의 확률 중 최대 확률의 index값(원소값)이 추출되는데, 이 때 axis=1인 이유는 각각의 이미지마다 10개씩 결과가  나와 2차원이 되므로 한 이미지 당 10개의 확률 결과끼리 비교하기 위함이다
    - 정답 레이블(t)도 2차원으로 y와 똑같이 계산되며, 마지막으로 맞은 개수(예측과 정답이 맞은 경우)의 총합을 전체 개수로 나누어 정확도를 구한다
```python
def accuracy(self, x, t):
    y = self.predict(x)
    y = np.argmax(y, axis=1)
    if t.ndim != 1:
        t = np.argmax(t, axis=1)

    accuracy = np.sum(y == t) / float(x.shape[0])
    return accuracy
```

- (앞에 ThreeLayerNet class에 이어지는 코드)
    - numerical_gradient는 매개변수(가중치와 편향)의 기울기를 수치 미분 방식으로 구하며, gradient는 오차 역전파로 구한다
    - MNIST 학습 시에는 오차 역전파로 진행하도록 했다
```python
def numerical_gradient(self, x, t):
    loss_W = lambda W: self.loss(x, t)

    grads = {}
    grads['W1'] = numerical_gradient(loss_W, self.params['W1'])
    grads['b1'] = numerical_gradient(loss_W, self.params['b1'])
    grads['W2'] = numerical_gradient(loss_W, self.params['W2'])
    grads['b2'] = numerical_gradient(loss_W, self.params['b2'])
    grads['W3'] = numerical_gradient(loss_W, self.params['W3'])
    grads['b3'] = numerical_gradient(loss_W, self.params['b3'])

    return grads
```
```python
def gradient(self, x, t):
    # 순전파
    self.loss(x, t)

    # 역전파 시작
    dout = 1
    dout = self.lastLayer.backward(dout)

    layers = list(self.layers.values())
    layers.reverse()
    for layer in layers:
        dout = layer.backward(dout)

    grads = {}
    grads['W1'], grads['b1'] = self.layers['Affine1'].dW, self.layers['Affine1'].db
    grads['W2'], grads['b2'] = self.layers['Affine2'].dW, self.layers['Affine2'].db
    grads['W3'], grads['b3'] = self.layers['Affine3'].dW, self.layers['Affine3'].db

    return grads
```

- 신경망 구현
```python
for i in range(iters_num):
    # 미니배치 생성
    batch_mask = np.random.choice(train_size, batch_size)
    x_batch = x_train[batch_mask]
    t_batch = t_train[batch_mask]

    # 기울기 계산 (역전파)
    grad = network.gradient(x_batch, t_batch)

    # 가중치 업데이트
    for key in ('W1', 'b1', 'W2', 'b2', 'W3', 'b3'):
        network.params[key] -= learning_rate * grad[key]

    # 손실 계산
    loss = network.loss(x_batch, t_batch)
    train_loss_list.append(loss)

    # 1 epoch 당 정확도 계산
    if i % iter_per_epoch == 0:
        epoch += 1
        train_acc = network.accuracy(x_train, t_train)
        test_acc = network.accuracy(x_test, t_test)
        train_acc_list.append(train_acc)
        test_acc_list.append(test_acc)
        train_loss_list2.append(loss)

        print(
            'Epoch:', epoch,
            '=> train 정확도:', round(train_acc * 100, 2), '%',
            '/ test 정확도:', round(test_acc * 100, 2), '%',
            '/ loss:', round(loss, 4)
        )

print("종료")
```

- 모델의 손실(loss)값 시각화
```python
import matplotlib.pyplot as plt

plt.figure(figsize=(15, 9))
plt.plot(train_loss_list2)
plt.title('model loss')
plt.ylabel('loss')
plt.xlabel('epoch')
plt.legend(['loss'], loc='upper left')
plt.show()
```

- 모델의 정확도 시각화
```python
import matplotlib.pyplot as plt

plt.figure(figsize=(15, 9))
plt.plot(train_acc_list)
plt.plot(test_acc_list)
plt.title('model accuracy')
plt.ylabel('accuracy')
plt.xlabel('epoch')
plt.legend(['train', 'test'], loc='upper left')
plt.show()
```

# 신경망 실습(2)
- MNIST Dataset
    - MNIST Dataset은 tensorflow 라이브러리에서도 불러올 수 있으며, Train set과 Test set으로 나누어져 있다
    - Train set을 확인해보면, 60000개가 있다
```python
import numpy as np
import tensorflow as tf
import matplotlib.pyplot as plt

import warnings
warnings.filterwarnings(action='ignore')

# MNIST Data 불러오기
mnist = tf.keras.datasets.mnist
(TrainX, TrainY), (TestX, TestY) = mnist.load_data()

TrainX.shape
```

- MNIST Dataset
    - Train set의 1번째 이미지를 확인해보면 pdf그림처럼 나온다
    - 그 중 오른쪽은 실제 숫자로 이루어진 그림이다
```python
image_0 = TrainX[0]
plt.imshow(image_0)
plt.show()
```

- 정규화 및 모델 구성
    - TrainX 와 TestX의 경우 학습 성능을 위해 정규화를 진행하는데, 이미지들이 0~255의 픽셀값으로 이루어져 있으므로 255로 나누어준 후 모델을 구성한다
    - 이전 실습과는 다르게 7~8줄 만으로 모델 구성을 할 수 있다
```python
TrainX, TestX = TrainX / 255.0, TestX / 255.0

# 모델 구성
model = tf.keras.models.Sequential([
    tf.keras.layers.Flatten(input_shape=(28, 28)),
    tf.keras.layers.Dense(128, activation='sigmoid'),
    tf.keras.layers.Dense(10, activation='softmax')
])

model.compile(
    optimizer='adam',
    loss='sparse_categorical_crossentropy',
    metrics=['accuracy']
)

model.summary()
```

- 모델 학습
    - 이렇게 한 줄 만으로 구성한 모델을 학습(순전파/역전파 수행)할 수 있다
```python
model.fit(TrainX, TrainY, epoch=10)
```

- 모델 성능 평가
    - 단 1줄만으로 학습한 모델의 성능을 평가할 수 있다
```python
loss, accuracy = model.evaluate(TestX, TestY, verbose=3)
print('loss값 : ', loss)
print('모델 정확도 : ', round(accuracy * 100, 3), '%')
```

