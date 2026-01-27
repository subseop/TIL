# 신경망 기초

## 신경망
- 인공 신경망
    - 인공 신경망은 인간의 두뇌를 구성하는 신경세포인 뉴런의 동작원리를 모방하여 만든 모델
    - 인간의 두뇌는 수많은 뉴런과 시냅스를 통해 병렬적으로 연결되어 있으며, 각각의 뉴런은 다른 뉴런으로부터 가지돌기를 통해 신호를 입력받아 축삭돌기를 통해 다른 뉴런으로 신호를 내보낸다
    - 출력신호는 입력된 신호가 모여서 임계치를 넘어설 때 발생

## 퍼셉트론
- 퍼셉트론(perceptron)
    - 퍼셉트론은 perception + neuron
    - 뉴런이 감각 정보를 받아서 문제를 해결하는 원리를 반영한 인공 뉴런, 인공신경망의 기초
    - 보통 퍼셉트론이라고 하면, 단층 퍼셉트론을 말한다
    - 입력 신호가 뉴런에 보내질 때 각각 고유한 가중치가 곱해지는데, 뉴런 신호의 총합이 정해진 한계를 넘어서면 1을 출력하고 뉴런이 활성화된다

    - 페셉트론은 복수의 입력 신호 각각에 고유한 가중치를 부여, 가중치는 각 신호가 결과에 주는 영향력을 조절하는 요소로 작용하여 가중치가 클수록 해당 신호가 더 중요하다는 뜻
    - 퍼셉트론은 입력 신호에 가중치를 곱한 값과 편향을 합하여 그 값이 0을 넘으면 1을 출력(활성화)하고 그렇지 않으면 0을 출력(비활성화)한다.

- 단층 퍼셉트론(single layer perceptron)
    - 아래 그림은 가중치를 갖는 층이 입력층 1개이기 때문에 단층 퍼셉트론이며, 2개의 신호를 받는 퍼셉트론이다  
    => 첫번째 그림은 bias(편향)이 없는 경우, 두번째는 있는 경우
    - 편향의 입력 신호는 항상 1이며, 편향과 곱해져 가중치가 곱해진 입력값들과 더해진다
    --------------------------------  
        x1[x₁] -- w₁ --> y                              
        x1[x₁] -- w₁ --> y                              
    --------------------------------                                                     
        b[1] -- b --> y 
        x2[x₁] -- w₂ --> y 
        x2[x₂] -- w₂ --> y  
    ---------------------------------
    - 입력신호가 뉴런에 보내질 때 각각 고유한 가중치가 곱해지는데 뉴런의 신호의 총합이 정해진 한계를 넘어서면 1을 출력하고, 뉴런이 활성화된다
    - 정해진 한계는 임계값이라고 하며, θ(세타)로 표현한다
    - 퍼셉트론은 복수의 입력 신호 각각에 고유한 가중치를 부여하는데 가중치는 각 신호가 결과에 주는 영향력을 조절하는 요소로 작용하여 가중치가 클수록 해당 신호가 더 중요하다는 뜻이다
    ------------------------------------
        x1[x₁] -- w₁ --> y                               
        x1[x₁] -- w₁ --> y  
    -------------------------------------  
        y = 0  (w₁x₁ + w₂x₂ ≤ θ)  
        y = 1  (w₁x₁ + w₂x₂ > θ)
    -------------------------------------
    - 이전의 식의 θ를 -b로 치환하면 아래의 식처럼 되며, b는 bias(편향)이다.
    - 식에서도 알 수 있듯이, 단층 퍼셉트론은 선형 구조이며, 퍼셉트론은 입력 신호에 가중치를 곱한 값과 편향을 합하여, 그 값이 0을 넘으면 1을 출력(활성화)하고 그렇지 않으면 0을 출력(비활성화)한다.  
    --------------------------------------
    b[1] -- b --> y 
    x2[x₁] -- w₂ --> y 
    x2[x₂] -- w₂ --> y 
    --------------------------------------
    y = 0  (w₁x₁ + w₂x₂ + b ≤ θ)  
    y = 1  (w₁x₁ + w₂x₂ + b > θ)
    ---------------------------------------

- 단층 퍼셉트론으로 논리 게이트 구현(AND, OR, NAND)
    - 1번째 코드가 단층 퍼셉트론으로 구현한 AND gate, 2번째 코드가 OR gate, 3번째 코드가 NAND gate
    - 세 gate의 식은 동일하지만, 각각의 gate 결과를 만족하는 가중치와 편향을 가지고 있다
```python
def AND_gate(x1, x2):
  w1 = 0.5
  w2 = 0.5
  b = -0.7
  result = x1*w1 + x2*w2 + b
  if result <= 0:
    return 0
  else:
    return 1
print('AND gate에 0, 0 입력 결과:', AND_gate(0, 0))
print('AND gate에 0, 1 입력 결과:', AND_gate(0, 1))
print('AND gate에 1, 0 입력 결과:', AND_gate(1, 0))
print('AND gate에 1, 1 입력 결과:', AND_gate(1, 1))
------------------------------------------------------
AND gate에 0, 0 입력 결과: 0
AND gate에 0, 1 입력 결과: 0
AND gate에 1, 0 입력 결과: 0
AND gate에 1, 1 입력 결과: 1
```
```python
def OR_gate(x1, x2):
  w1 = 0.6
  w2 = 0.6
  b = -0.5
  result = x1*w1 + x2*w2 + b
  if result <= 0:
    return 0
  else:
    return 1
print('OR gate에 0, 0 입력 결과:', OR_gate(0, 0))
print('OR gate에 0, 1 입력 결과:', OR_gate(0, 1))
print('OR gate에 1, 0 입력 결과:', OR_gate(1, 0))
print('OR gate에 1, 1 입력 결과:', OR_gate(1, 1))
--------------------------------------------------
OR gate에 0, 0 입력 결과: 0
OR gate에 0, 1 입력 결과: 1
OR gate에 1, 0 입력 결과: 1
OR gate에 1, 1 입력 결과: 1
```
```python
def NAND_gate(x1, x2):
  w1 = -0.5
  w2 = -0.5
  b = 0.7
  result = x1*w1 + x2*w2 + b
  if result <= 0:
    return 0
  else:
    return 1
print('NAND gate에 0, 0 입력 결과:', NAND_gate(0, 0))
print('NAND gate에 0, 1 입력 결과:', NAND_gate(0, 1))
print('NAND gate에 1, 0 입력 결과:', NAND_gate(1, 0))
print('NAND gate에 1, 1 입력 결과:', NAND_gate(1, 1))
------------------------------------------------------
NAND gate에 0, 0 입력 결과: 1
NAND gate에 0, 1 입력 결과: 1
NAND gate에 1, 0 입력 결과: 1
NAND gate에 1, 1 입력 결과: 0
```

## 다층 퍼셉트론의 한계계
- 단층 퍼셉트론의 한계(XOR)
    - 단층 퍼셉트론으로 AND, OR, NAND gate는 구현할 수 있지만, XOR gate는 구현하지 못한다
    - 직선 한 개로 AND와 OR은 분류할 수 있지만 XOR은 불가능

- 단층 퍼셉트론의 한계점 해결법
    - 곡선으로 분류한다(비선형 함수 사용)
    - 다층 퍼셉트론을 이용한다

## 다층 퍼셉트론의 등장
- 다층 퍼셉트론(multi layer perceptron)
    - 다층 퍼셉트론은 층을 여러 개 쌓은 퍼셉트론을 의미한다
    - 층을 여러개 쌓아서 깊게 늘려가면, 더 다양한 것들을 표현할 수 있다

- 다층 퍼셉트론으로 XOR 구현하기
    - XOR 게이트는 AND, NAND, OR 게이트를 조합하여 구현할 수 있다
    - 이처럼 단층 퍼셉트론으로는 표현하지 못한 것을 다층 퍼셉트론으로 구현할 수 있다
```python
def XOR_gate(x1, x2):
  s1 = NAND_gate(x1, x2)
  s2 = OR_gate(x1, x2)
  y = AND_gate(s1, s2)
  return y

print('XOR gate에 0, 0 입력 결과:', XOR_gate(0, 0))
print('XOR gate에 0, 1 입력 결과:', XOR_gate(0, 1))
print('XOR gate에 1, 0 입력 결과:', XOR_gate(1, 0))
print('XOR gate에 1, 1 입력 결과:', XOR_gate(1, 1))
---------------------------------------------------
XOR gate에 0, 0 입력 결과: 0
XOR gate에 0, 1 입력 결과: 1
XOR gate에 1, 0 입력 결과: 1
XOR gate에 1, 1 입력 결과: 0
```

## 활성화 함수
- 활성화 함수의 필요성
    - 하지만, 다층 퍼셉트론처럼 층을 무작정 쌓기만 한다고 해서 퍼셉트론을 선형분류기에서 비선형분류기로 바꿀 수 있는 것은 아니다
    - 선형 시스템의 경우 층을 아무리 깊게 쌓아도 깊게 쌓는 의미가 없어지기 때문이다

    - 예를 들어, 선형 함수인 h(x) = ax 를 사용하는 3층의 네트워크가 있다고 가정하자.
    - y(x) = a(a(ax)) = a³x 가 되는데, 이는 y(x) = ax와 같은 꼴이다
    - 즉, 이는 은닉층이 없는 처음과 같은 네트워크로 표현되기 때문에 층을 깊게 하는 의미가 없어진다
    - 이러한 문제점을 해결해주는 것이 바로 활성화 함수이다ㅣ

- 활성화 함수
    - 생물학적 뉴런은 한 개의 신호(입력)가 아니라 여러 신호를 받는데, 신호르 받을 때마다 매번 반응(출력)할 수 없어 여러 신호의 합들이 임계치를 넘어야만 반응하도록 되어있다
    - 딥러닝의 신경망에서는 활성화 함수가 이러한 특성을 재현하며, 활성화 함수에 입력 값들의 합을 전달하면, 해당 입력에 대해 활성화/비활성화를 결정한다  
    + 입력들의 가중합에 편향을 더한 값이 활성화 함수에 입력 -> 활성화 함수의 형태에 따라 출력이 결정

- 단층 퍼셉트론에서의 활성화 함수(계단 함수)
    - 아래 식은 단층 퍼셉트론 식에서 활성화 함수가 적용된 형태이다
    - w₁x₁ + w₂x₂ + b 는 입력 신호의 총합이며, 간단히 a로 치환된다
    - 입력 신호의 총합 a는 h(a)라는 활성화 함수를 거쳐 y로 출력되며, 활성화 함수인 h(a)함수는 입력이 0을 넘으면 1을 출력하고, 그렇지 않으면 0을 출력한다
    ------------------------------------    
        y = h(w₁x₁ + w₂x₂ + b) 
    ------------------------------------   
        y = 0  (w₁x₁ + w₂x₂ + b ≤ θ)  
        y = 1  (w₁x₁ + w₂x₂ + b > θ)
    ------------------------------------
        h(a) =  0  (a ≤ 0)  
                1  (a > 0)
    ------------------------------------
    - 가중치 신호를 조합한 결과가 a이고, a는 활성화 함수를 거쳐 0또는 1로 출력
    h(a)는 계단형태의 비선형 함수이다
    - 하지만, 계단 함수는 2가지 단점으로 인해 신경망에는 시용되지 않으며, 신경망에서는 보통 sigmoid(시그모이드) 함수를 활성화 함수로 사용한다

- 계단 함수의 단점(1)
    - 불연속성(미분 불가능)
        - 계단 함수는 x=0에서 불연속적이다
        - x=0에서 불연속적이라는 얘기는 미분이 불가능한 지정이라는 뜻이므로 해당 지점에서는 신경망을 학습할 수 없다
- 계단 함수의 단점(2)
    - 기울기 소실(gradient vanishing)
        - 기울기 소실의 뜻은 기울기가 0이 되어 소실된다는 의미이다
        - x=0을 제외한 나머지 지점들에서 미분값이 0이다
        - 미분값이 0인 지점은 학습이 잘 된 경우(cost function이 최소 지점)일 수도 있지만 제대로 학습이 되지 않는 경우일 수도 있다

- sigmoid(시그모이드)함수
    - sigmoid는 's자 모양'이라는 뜻으로 시그모이드 함수는 기본적으로 s모양을 그리는 곡선 함수를 통칭하여 부르는 말이다
    - 대표적인 sigmoid함수로는 logistic(로지스틱)함수와 tanh(하이퍼탄젠트)함수가 있다

- ReLU(Rectified Liner Unit) 함수
    - ReLU 함수는 이러한 기울기 소실 문제를 극복하기 위해서 등장한 함수이다
    - ReLU 함수는 x가 0보다 클 때, 미분값이 항상 1이므로 층이 아무리 깊어져도 손실 없이 정보를 전달할 수 있다
    - 미분값이 항상 0과1이기 때문에 연산이 빠르다는 장점이 있다
    - 이러한 점들로 인해 ReLU 함수는 신경망에서 많이 사용된다
    --------------------------------------
    h(a) =  0  (a ≤ 0)  
            a  (a > 0)  
    ----------------------------------------
- Leaky ReLU(Rectified Linear Unit) 함수
    - 하지만, ReLU함수 역시 단점이 존재하는데, x=0 이하의 값에서는 기울기 소실 문제가 발생한다
    - 이러한 문제를 해결하는 함수가 leaky ReLU함수이다
    - Leaky ReLU 함수는 x=0이하의 값에서 기울기 소실이 발생하지 않도록 하며, 보통 기울기는 0.01로 설정한다
    - 하지만, leaky ReLU함수는 ReLU 함수에 비해 연산의 복잡성이 크므로 ReLU가 더 많이 사용된다
    --------------------------------------
    h(a) =  0.01a  (a ≤ 0)  
            a      (a > 0)  
    ----------------------------------------

- Softmax 함수
    - 이진 분류의 경우에는 입력값을 받아 0혹은 1의 값을 출력하는 것이므로 주로 logistic 함수를 활성화 함수로 많이 사용한다
    - 하지만, 다중 클래스를 분류해야 하는 경우 logistic 함수는 제대로 분류를 하지 못하기 때문에 softmax 함수를 활성화 함수로 사용한다  
    => 전체 확률이 1일때, 각 클래스의 확률의 값이 0~1사이로 제한하고 총합을 1로 설정

- Softmax 함수 구현 시 문제점
    - Softmax함수의 경우 지수 함수를 사용하기 때문에 큰 값이 입력으로 들어오면 출력값이 증폭하게 되어 무한대로 발산하는 경우가 있다
```python
import numpy as np

def softmax_function(x):
  exp_x = np.exp(x)
  sum_exp_x = np.sum(exp_x)
  y = exp_x / sum_exp_x

  return y

input_array = [1000, 1000, 1002]

result = softmax_function(input_array)

print('결과:', result)
----------------------------------------
결과: [nan nan nan]
```

- Softmax 함수 구현 시 문제점 해결법
    - 따라서, 이러한 경우는 입력 신호 중 최대값을 빼주면 된다.  
    => 아래 예제에서 최대값은 1002이다
```python
import numpy as np

def softmax_function(x):
  input_max = np.max(x)
  print('입력 최대값:', input_max)
  exp_x = np.exp(x - input_max)
  sum_exp_x = np.sum(exp_x)
  y = exp_x / sum_exp_x

  return y

input_array = [1000, 1000, 1002]

result = softmax_function(input_array)

print('결과:', result)
-----------------------------------------
입력 최대값: 1002
결과: [0.10650698 0.10650698 0.78698604]
```

## 신경망망
- 다층 퍼셉트론(신경망) 학습
    - 다층 퍼셉트론을 통해 다양한 것들을 표현할 수 있지만 가중치를 설정하는 작업은 여전히 수동으로 직접 해야 한다는 문제점이 있다
    - 방금 전에 구현한 XOR의 경우도 단층 퍼셉트론에서 가중치 설정이 완료된 상태에서 했기 때문에 따로 가중치를 설정하지 않았지만, 원래는 직접 가중치를 수동으로 설정해야 한다
    - 따라서, 우리는 신경망 학습 방법을 통해 데이터에 가장 잘 맞는 가중치를 수동으로 직접 찾는 것이 아니라 자동으로 찾도록 한다

- 신경망
    - 신경망은 일반적으로 입력층, 은닉층, 출력층으로 구분된다
    - 신경망 역시 다양한 형태로 확장이 가능하며, 은닉층을 더 추가할 수도 있다
    - 은닉층 안에 활성화 함수가 있고, 은닉층(1), 은닉층(2), 출력층으로 갈 때마다 편향이 더해진다

- 신경망의 학습
    - 신경망은 데이터에 가장 잘 맞는 가중치를 자동으로 찾도록 한다
    - 이를 위해 신경망은 순전파 알고리즘과 역전파 알고리즘을 활용한다
    - 순전파는 순방향 전파라는 뜻으로 입력 데이터가 여러 층의 신경망을 따라 신호를 전파하면서 최종 출력을 만들어 가는 과정을 말한다
    - 역전파는 역방향 전파라는 뜻으로 순전파에서 발생한 오차를 줄이기 위해 출력층에서 입력층 방향으로 계산하면서 가중치를 업데이트하는 과정이다

- 신경망의 순전파(forward propagation)
    - 먼저, 입력층에서 은닉층(1)로 신호를 전달한다
    - 맨 처음의 가중치는 임의로 설정한다
    - 은닉층(1)의 값들에 각각 모두 계산

    - 그 다음 은닉층(1)에서 은닉층(2)로 신호를 전달
    - 은닉층(2)도 모두 각각 계산

    - 그 다음 은닉층(2)에서 출력층으로 신호를 전달한다
    - 출력층도 모두 각각 계산  
    => 신경망의 순전파는 결국 행렬 연산의 무수한 반복 -> GPU 중요

# 신경망 실습

## 단층 퍼셉트론을 통한 놀리 게이트 구현
- 단층 퍼셉트론으로 논리 게이트 구현(AND, OR, NAND)
    - 위에서 했음
- 다층 퍼셉트론으로 XOR 구현하기
    - 위에서 했음

## 활성화 함수
- 계단 함수
    - 계단 함수는 x축이 0인 시점을 경계로 출려깅 0에서1(또는 1에서 0)으로 바뀐다
```python
import numpy as np

def step_function(x):
  y = x > 0
  return y.astype(int)
```
```python
import matplotlib.pyplot as plt

x = np.arange(-5, 5, 0.1)
y = step_function(x)
plt.plot(x, y)
plt.ylim(-0.1, 1.1)
plt.show()
```

- sigmoid 함수(logistic 함수)
    - 구현된 sigmoid식은 logistic 함수이며, np.exp(-x)는 exp(-x)수식에 해당
```python
import numpy as np

def logistic_function(x):
  return 1/(1+np.exp(-x))
```
```python
x = np.arange(-5, 5, 0.1)
y = logistic_function(x)
plt.plot(x, y)
plt.ylim(-0.1, 1.1)
plt.show()
```

- sigmoid 함수(tanh 함수)
    - 구현된 sigmoid식은 tanh 함수이며, np.exp(x)는 exp(x). np.exp(-x)는 exp(-x) 수식에 해당
```pyton
def tanh_function(x):
  return (np.exp(x)-np.exp(-x))/(np.exp(x)+np.exp(-x))
```
```python
x = np.arange(-5, 5, 0.1)
y = tanh_function(x)
plt.plot(x, y)
plt.ylim(-1.1, 1.1)
plt.show()
```

- ReLU 함수
    - ReLU함수는 x가 0보다 클 때, 미분값이 항상 1이므로 층이 깊어져도 손실 없이 정보를 전달할 수 있는 반면, x가 0보다 작다면 기울기 소실이 발생한다
```python
def ReLU_function(x):
  return np.maximum(0, x)
```
```python
x = np.arange(-5, 5, 0.1)
y = ReLU_function(x)
plt.plot(x, y)
plt.show()
```

- Leaky ReLU 함수
    - Leaky ReLU 함수는 x=0 이하의 값에서 기울기 소실이 발생하지 않도록 하며, 보통 기울기는 0.01로 설정한다
```python
def Leaky_ReLU_function(x):
  return np.maximum(0.01*x, x)
```
```python
x = np.arange( -5, 5, 0.1)
y = Leaky_ReLU_function(x)
plt.plot(x, y)
plt.show()
```

- Softmax 함수
    - Softmax 함수는 다중 클래스를 분류해야 하는 경우 사용한다
```python
def softmax_function(x):
  exp_x = np.exp(x)
  sum_exp_x = np.sum(exp_x)
  y = exp_x / sum_exp_x
  return y
```
```python
x = np.arange( -5, 5, 0.1)
y = softmax_function(x)
plt.plot(x, y)
plt.show()
```
- 무한대로 발산하는 문제점을 해결하는 softmax 함수도 그래프 모양은 같다
```python
def softmax_function(x):
  input_max = np.max(x)
  exp_x = np.exp(x - input_max)
  sum_exp_x = np.sum(exp_x)
  y = exp_x / sum_exp_x
  return y
```

## 신경망 순전파
- 신경망 순전파 1단계
    - 코드에서 a1은 그림에서 a₁^(1), a₂^(1), a₃^(1)을 모두 포함하며,
    z1은 z₁^(1), z₂^(1), z₃^(1)을 모두 포함한다.
```python
def sigmoid_function(x):
  return 1/(1+np.exp(-x))

x = np.array([1.0, 0.5])
w1 = np.array([[0.1, 0.3, 0.5], [0.2, 0.4, 0.6]]) # 앞은 x1 가중치, 뒤는 x2 가중치
b1 = np.array([0.1, 0.2, 0.3])

print('x의 형태(모양):', x.shape)
print('w1의 형태(모양):', w1.shape)
print('b1의 형태(모양):', b1.shape)

a1 = np.dot(x, w1) + b1
z1 = sigmoid_function(a1)

print('a1:', a1)
print('z1:', z1)
--------------------------------------------------------------------------
x의 형태(모양): (2,)
w1의 형태(모양): (2, 3)
b1의 형태(모양): (3,)
a1: [0.3 0.7 1.1]
z1: [0.57444252 0.66818777 0.75026011]
```

- 신경망 순전파 2단계
    - 코드에서 코드에서 a2은 그림에서 a₁^(2), a₂^(2)을 모두 포함하며,
    z2은 z₁^(2), z₂^(2)을 모두 포함한다.
```python
w2 = np.array([[0.1, 0.4], [0.2, 0.5], [0.3, 0.6]])
b2 = np.array([0.1, 0.2])

print('w2의 형태(모양):', w2.shape)
print('b2의 형태(모양):', b2.shape)

a2 = np.dot(z1, w2) + b2
z2 = sigmoid_function(a2)

print('a2:', a2)
print('z2:', z2)
----------------------------------------------------------------------
w2의 형태(모양): (3, 2)
b2의 형태(모양): (2,)
a2: [0.51615984 1.21402696]
z2: [0.62624937 0.7710107 ]
```

- 신경망 순전파 3단계
    - 코드에서 a3은 그림에서 a₁^(3), a₂^(3)을 모두 포함하며, y은 y₁, y₂를 모두 포함한다
```python
w3 = np.array([[0.1, 0.3], [0.2, 0.4]])
b3 = np.array([0.1, 0.2])

a3 = np.dot(z2, w3) + b3
y = sigmoid_function(a3)

print('a3:' ,a3)
print('y(최종출력값):', y)
--------------------------------------------------------------------------
a3: [0.31682708 0.69627909]
y(최종출력값): [0.57855079 0.66736228]
```

## MNIST
- MNIST
    - dataset 디렉토리가 같은 경로에 있다면 MNIST데이터를 load_mnist로 load할 수 있으며 normalization=True를 통해 정규화하고, 이미지를 연산하기 위해 flatten시킨다
    - lnit_network 함수는 pickle파일인 sample_weight.pkl에 저장된 학습된 네트워크(가중치,편향)를 load한다다
```python
import numpy as np
import pickle                       # 파이썬 객체를 파일로 저장/불러오기 위한 라이브러리리
from dataset.mnist import load_mnist # mnist 데이터셋을 불러오는 함수

(x_train, y_train), (x_test, y_test) = \    
    load_mnist(normalize=True, flatten=True) 
# normalize=True는 픽셀값을 0~255 -> 0~1로 변환 즉, 정규화
# flatten=True는 (28x28) 이미지를 1차원 벡터(784)로 변환

def sigmoid_function(x):
    return 1 / (1 + np.exp(-x))

def softmax_function(x):
    exp_x = np.exp(x)
    sum_exp_x = np.sum(exp_x)
    y = exp_x / sum_exp_x
    return y

def init_network():                         # 신경망을 초기화하는 함수
    with open("sample_weight.pkl", "rb") as f: # sample_weight.pkl 파일 읽기
        network = pickle.load(f) # 파일 안에 저장된 신경망 가중치 딕셔너리를 메로리로 로드
    return network
```

- MNIST
    - predict 함수에서는 입력받은 network를 바탕으로 순전파에 대한 연산을 진행한다
    - 이전의 순전파 실습에서는 sigmoid함수만 적용했지만 MNIST 실습에서는 마지막에 softmax 함수를 적용한다다
```python
def predict(network, x):
    W1, W2, W3 = network['W1'], network['W2'], network['W3']
    b1, b2, b3 = network['b1'], network['b2'], network['b3']
    # 가중치와 편향 꺼내기
    a1 = np.dot(x, W1) + b1
    z1 = sigmoid_function(a1)

    a2 = np.dot(z1, W2) + b2
    z2 = sigmoid_function(a2)

    a3 = np.dot(z2, W3) + b3
    y = softmax_function(a3)

    return y
```
```
각각의 MNIST 이미지(즉, 이미지 1개)에 대한 W1 : (784, 50)
각각의 MNIST 이미지(즉, 이미지 1개)에 대한 W2 : (50, 100)
각각의 MNIST 이미지(즉, 이미지 1개)에 대한 W3 : (100, 10)
각각의 MNIST 이미지(즉, 이미지 1개)에 대한 b1 : (50,)
각각의 MNIST 이미지(즉, 이미지 1개)에 대한 b2 : (100,)
각각의 MNIST 이미지(즉, 이미지 1개)에 대한 b3 : (10,)
```

## 신경망 구조
```python
print(np.shape(x_test))
print(np.shape(x_test[0]))   # 784 = 28*28

print(np.shape(network['W1']))
print(np.shape(network['b1']))

print(np.shape(network['W2']))
print(np.shape(network['b2']))

print(np.shape(network['W3']))
print(np.shape(network['b3']))
-----------------------------------------
(10000, 784)
(784,)
(784, 50)
(50,)
(50, 100)
(100,)
(100, 10)
(10,)
```

- MNIST
    - predict 함수의 구조
        - 입력층의 뉴런(노드)의 개수는 MNIST 이미지 하나의 크기가28*28이므로 784개이다
        - 은닉층은 2개이며 각 은닉층의 뉴런 개수는 50개와 100개로 정했다
        - 출력층의 뉴런 개수는 0~9까지의 숫자를 구분하는 문제이기 때문에 10개이다

    - 아까 선언했던 init_network 함수를 실행하면, 이렇게 미리 저장된 가중치와 편향을 확인할 수 있다
```python
network = init_network()
print('미리 저장된 네트워크(가중치, 편향) : ', network, sep='\n')
----------------------------------------------------------------------------------
미리 저장된 네트워크(가중치, 편향) :
{'b2': array([-0.01471108, -0.07215131, -0.00155692,  0.12199665,  0.11603302,
              -0.00754946,  0.04085451, -0.08496164,  0.02898045,  0.0199724 ,
               0.19770803,  0.04865116, -0.06518728, -0.05226324,  0.0113163 ,
               0.03049979,  0.0460355 ,  0.0695399 , -0.07778469,  0.0692313 ,
               0.09365533,  0.0540801 , -0.03843745,  0.02123107,  0.03793406,
               ...
              -0.00576806, -0.09652461, -0.05131314,  0.02199687, -0.04358608])}
```

- MNIST
    - 예측값을 확인하고 싶다면, print를 실행하면 된다.
    - 
```python
accuracy_cnt = 0

for i in range(len(x_test)):    # 테스트 데이터 전체 반복 len(x_test) = 10000
    y = predict(network, x_test[i]) # x_test[i] : 이미지 1장(784차원)
    p = np.argmax(y)                # argmax는 가장 큰 값의 인덱스 선택
    if p == y_test[i]:              # p는 모델 예측값, y_test[i]는 실제 정답
        accuracy_cnt += 1           # 맞힌 경우만 +1

print("정확도 : " + str(round((float(accuracy_cnt) / len(x_test)) * 100, 3)) + "%")
# 정확도 계산 
# 1. accuracy_cnt / len(x_test) -> 비율
# 2. *100 -> 퍼센트
# 3. round(..., 3) -> 소수점 3자리리
------------------------------------------------------------------------------------
정확도 : 93.52%
```


- 추가로 원래의 신경망 학습 순서는 이렇게 됨
(1) 가중치 랜덤 초기화  
(2) 예측  
(3) 오차 계산 (loss)  
(4) 오차를 거꾸로 전달 (backpropagation)  
(5) 가중치 업데이트  
(6) 반복  
- MNIST는 이미 학습이 끝난 모델을 사용하는 실습