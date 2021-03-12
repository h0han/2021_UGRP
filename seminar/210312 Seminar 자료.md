# 210312 Seminar 자료

### CS231n Study 질문목록 및 답변 정리

#### Lec1-3

***Q. L2 규제가 좌표계에 독립적인 이유? & Lecture에서 L1 norm은 coordinate dependent하고, L2 norm은 coordinate dependent하지 않다고 하였다. 그림으로는 어느 정도 이해가 되는데, 수식적으로 살펴보면 잘 납득이 가질 않는다.***

A: 두 점 사이의 거리를 구하는 과정을 직관적으로 이해해봤을 때 l1은 좌표계를 이루는 기저벡터에 의해 결정되지만 l2는 기저벡터 즉 좌표계와 상관없이 거리가 동일하므로 L2 규제는 좌표계에 독립적이다. 즉 어떤 한 Feature에 종속된 것이 아니라 전체적인 Feature에 규제를 줄 수 있다.

#### Lec4

***Q. Backpropagation에서 Chain Rule을 적용하는 과정에서, 예를 들어 Y = WX에서 Y를 X로 미분하는 것을 어떻게 계산하는 것인지 궁금하다.  Y, W, X가 모두 행렬이면 스칼라 함수처럼 단순히 y를 x로 미분하면 w라고 하는 것이 불가능할 것 같다.***

#### Lec5

***Q. Image의 Spatial한 정보를 분석할 때 목표 구역이외의 정보들은 어떻게 제거하는가? 학습하는 과정에서 자연스레 Unactivate되는 것인가?***

A: CNN을 Train하는 과정에서 자연스레 무의미한 값들을 무시하도록 Update됨. 즉 무의미한 정보들을 가지지 않도록 Filter의 값들이 조절된다거나 Forward 될 때 불필요한 정보들을 Deactivate 되도록함.

***Q. Image 처리 시 Channel를 3개이상 쓰는 경우와 용도***

***Q.실제로 CNN에서 Padding을 사용하는 사례가 많지는 않은 것 같은데, 그 이유가 궁금하다.***

#### Lec6

***Q. ReLU 동작을 잘하기 위해 Xavier 초기화에서 분모를 2로 나눠주는 이유***

A: Xavier 초기화의 분모항에서 fan_in에 2를 나누어 주는 것은 궁극적으로 W값을 sqrt(2)를 곱해주는 것을 의미한다. 따라서 보다 넓은 분포를 가지게 하여 ReLU로 인한 Data 분포 소실 문제를 막는다.(논리적으로 정확한 이유는 잘 모르겠습니다.)

***Q. Xavier Initialization의 정확한 의미와 기능??***

A: 각각의 가중치들을 평균이 0이고 분산이 1인 가우시안 분포에서 fan_in * fan_out size의 행렬에 값을 할당하고 각각의 원소들을 sqrt(fan_in)으로 나누어 준다. 이때 node각각의 dot profuct들은 fan_in에 영향을 받는다. 즉 fan_in 값이 크면 dot_product들이 커질 수 있고 Saturate되어 마찬가지로 Gradient 소실문제를 가져올 수 있다. 이러한 문제를 방지하고자 가중치에 sqrt(fan_in)을 나누어 줌으로서 값이 Saturate 되는 것을 막을 수 있다.

***Q. ReLU에 positive bias를 주는 경우 Leaky ReLU와 비슷한 형태가 될 것 같은데 왜 도움이 안 되는지? 도움이 안되는 Leaky ReLU의 의미가 있는지?***

***Q. zero-mean 문제를 해결하려 각 data마다 normalization하는 것보다 dataset의 mean과 variance만큼 activation function을 조정하면 되는 것 아닌가?***

***Q. Saturation이 무조건 안 좋은 것인지?***

***Q. Decay의 의미***

***Q. 학습에서 Batch를 이용하면 매번 조금씩 다른 데이터를 가지고 학습하기 때문에 regularization 효과가 있다. 그런데 Batch Normalization의 경우, 매 학습마다 달라질 수 있는 신경망 내부의 데이터 분포를 어느 정도균일하게 함으로써 regularization의 효과를 보인다고 한다. 두 메커니즘이 상충되는 것 같은데, Batch Normalization의 정확한 원리(Internal Covariate Shift 등)와 어떻게 Regularization 효과를 얻는 것인지를 자세하게 알고 싶다.***

#### Lec7

***Q. Nesterov Momentum에서 의 정확한 의미(f 괄호 안 항들의 의미)***

A: SGD + Momentum은 현재 위치에서의 Gradient Vector와 Velocity Vector를 계산하여 그들의 벡터합으로 Descent를 진행하였다면 Nesterov Momentum은 velocity vector가 가리키는 지점에서의 gradint vector를 미리 계산하여 그 둘을 합한 방향으로 Descent를 진행하게 되는데 괄호 안에 항은 velocity vector로 인해 미리 예측된 지점을 의미한다.

#### Lec8

***Q. Static Graph(Tensorflow)와 Dynamic Graph(PyTorch)의 차이***

A: Static Graph(Tensorflow)

- 장점
  - 동일한 그래프를 계속 재사용하므로 최적화에 유리, 초기에는 최적화과정때문에 느릴 수 있지만, 반복이 진행될수록 효율성이 극대화됨
  - 전체 그래프의 자료구조를 파일형태로 저장가능하기 때문에 해당 파일에만 접근해 구동가능
- 단점
  - Computational Graph를 구성할 때 모든 경우의 수를 다 고려해야하므로 조건분기코드를 작성할 때 Python에서 제공되는 조건문이 아니라 따로 구성해야한다.

Dynamic Graph(PyTorch)

- 장점:
  - 코드가 쉽고 깔끔
  - 파이썬 조건문을 쓸수 있다.
  - 매 Iteration 마다 새로운 그래프를 만들기 때문에 경로를 나누어 놓아도 매번 선택할 수 있다.
  - 재귀적인 Loop에서도 이러한 장점이 발휘된다.
- 단점

***Q. Tensorflow에서 session.run을 했을 때 Output을 어떻게 지정할 수 있는가?***

A: Tensorflow 자체적으로 연산을 부르면 Computational Graph가 만들어지고 Session에서 argument로 반환하고자 하는 값들을 지정하면 그에 대한 Computational Graph에서 연산이 수행되고 결과값을 반환한다.

#### Lec9

***Q. model의 Parameter들은 어디에서 관리되는가? (Memory에서 관리되지 않는 가?)***

***Q. BottleNeck Layer을 사용한다는 것의 정확한 의미는?***

***Q. 계산량을 줄이기 위해 1x1conv를 사용하는데 왜 이전 filter의 수를 줄이지 않고 1x1 conv를 사용하는가?***

***Q. GoogLeNet에서 FC층을 없애는 데 FC 층의 역할은 어떻게 수행하는가?***

***Q. GoogLeNet에서 보조분류기를 통해 추가적인 Gradient를 얻을 수 있는 원리***

***Q. ResNet에서 H(x) - x를 학습한다는 의미와 장점?***

#### Lec10

***Q. argmax로 선택해서 가중치를 업데이트하는 것과 확률분포로부터 샘플링해서 업데이트하는 것이 무슨 차이인지? 샘플링하면 다양한 출력을 얻어낼 수 있어서 좋다고 하였는데, 어차피 loss는 정답에서 예측값을 빼서 계산하는 것 아닌가?***

***Q. 어텐션은 특정 단어 생성 시 이미지의 어느 부분을 봐야 하는지를 학습하는 것인지? 한 번도 보지 못한 test 데이터로 어텐션 과정을 거치면 모델이 어텐션할 부분을 정확히 찾아낼지?***