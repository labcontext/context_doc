# Normalization

#### 1. Batch normalization 

<p align="center">
   <img src="https://user-images.githubusercontent.com/26568793/57982037-80fa5900-7a7a-11e9-9058-c3b28cdc5308.png">
</p>

각 층의 출력 값을 정규화하는 방법, DNN은 학습의 층이 깊어지면 깊어질수록 weight값의 변화가 가중돼서 쌓이면 값의 변화가 커지기 때문이다. 즉, hidden layer 내의 값의 변화가 큰 것이 문제가 되는 것이다. 처음 학습했던 노드의 분포와 다음에 학습하는 분포가 다르다면 학습이 잘 안된다. 이게 바로 ‘Internal Covariate Shift’이다. 이를 해결하기 위해서 Batch norm이 제안되었는데, BN은 신경망 안에 포함되어 training 시에 평균과 분산을 조정하는 과정 역시 같이 조정이 된다. 

[[다시 이해하기](https://de-novo.org/2018/05/28/batch-normalization-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0/)]

<p align="center">
   <img src="https://user-images.githubusercontent.com/26568793/57982039-88b9fd80-7a7a-11e9-9778-7ed7b6dec5db.png">
</p>

W1은 X를 바탕으로 학습, 복습, 업데이트 된다. X는 proprocessing되어 normalized input이므로 layer “A”는 비슷한 input distribution을 가진다. 따라서 안정적으로 W1을 학습할 수 있다. 

<p align="center">
   <img src="https://user-images.githubusercontent.com/26568793/57982042-91aacf00-7a7a-11e9-8ae8-a1a4929a8395.png">
</p>

W2는 a를 바탕으로 학습, 업데이트 된다. f(XW1)은 W1의 값에 따라 분포가 달라진다. 즉, layer B의 input distribution은 고정되어 있지않다. 따라서 안정적으로 W2를 학습할 수 없다.  

학습 과정에서 가중치의 값이 ![image](https://user-images.githubusercontent.com/26568793/57982048-a2f3db80-7a7a-11e9-9bcd-c0202e5b8cac.png)로 업데이트되면 이전 레이어의 activation 또한  ![image](https://user-images.githubusercontent.com/26568793/57982048-a2f3db80-7a7a-11e9-9bcd-c0202e5b8cac.png)로 바뀌게 된다. 은닉층의 입장에서는 인풋 값의 분포가 계속 널뛰는 것이나 마찬가지이다. 입력 분포의 형태가 유지되지 않으므로 학습도 잘 진행되지 않는다. 그라디언트 값이 큰 학습 초기일수록 문제가 더 심각해진다.

바로 해당 문제가 ‘Internal covariate shift’라고 한다. 말 그대로 입력층보다 깊은( internal에 있는) 입력값은 공변량(covariate)이 고정된 분포를 갖지 않고 이리저리 움직(shift)인다는 것이다. 

이를 해결하기 위해서 BN을 사용한다. 

해당 문제를 해결하기 위해서는 초반 입력데이터를 표준화 했던 것처럼 은닉층의 입력도 표준화한다면 안정적으로 깊은 레이어의 가중치 역시 학습시킬 수 있을 것이다. 즉 이는 이전 층의 출력(raw activation)을 표준화 한다는 의미와 같다.  

![image](https://user-images.githubusercontent.com/26568793/57982045-9a030a00-7a7a-11e9-96d6-5765e1f47258.png)

딥러닝은 거의 항상 전체의 sample을 mini batch로 나누어 학습을 진행하여 가중치를 업데이트 하기 때문에 이전층의 출력을 표준화 할 때도 각 batch마다 따로 표준화를 진행하면 된다. 따라서 mini batch의 평균과 표준편차로 표준화한 activation 값을 은닉층 B의 입력으로 사용하면 은닉층B 역시 고정된 분포로 학습을 진행할 수 있다. 하지만 이런 식으로 은닉층의 입력을 표준화 하면 gradient update 과정에서 bias 값이 무시된다. 따라서 bias대신 평향의 역할을 할 파라미터가 추가되어야 한다. 또한 raw activation의 분포를 고정하는 것이 좋긴하지만 항상 N(0,1)로 고정할 필요는 없다. 적절하게 scaling, shifting된 activation을 사용하는 것이 학습에 도움이 될 수 있다. 따라서 해당 방식을 이용하여 activation function의 입력으로 사용하는 것을  BatchNorm이라고 한다. 

--------------------------

#### 2. [Layer Normalization](<https://www.slideshare.net/ssuser06e0c5/normalization-72539464>) 



<p align="center">
   <img src="https://user-images.githubusercontent.com/26568793/58149805-6228dc00-7c9f-11e9-9cd0-ac33ce5538f4.png">
</p>

해당 Normalization의 목표는 state-of-art한 DNN의 학습시간을 단축시키고 싶어 중간층의 출력을 정규화 함으로서 실현하고자 한다. 해당 방식은 Batch size에 의존하지 않아 온라인 학습이나 작은 mini-batch도 사용할 수 있다. 또한 Train과 Test의 계산 방법이 동일하며 그대로 RNN에 적용이 가능하다. (이 모두가 BN에서의 한계점이었다.)  채널과 영상전체를 모두 normalize
시켜주는 기술. LN은 mini-bath의 feature의 수가 같아야 한다.

<p align="center">
   <img src="https://user-images.githubusercontent.com/26568793/58149993-31957200-7ca0-11e9-9a65-0afe1baa4af1.png">
</p>

BN과 LN의 차이점은 아래의 그림과 도표를 참고하면 된다. 

<p align="center">
   <img src="https://user-images.githubusercontent.com/26568793/58150227-edef3800-7ca0-11e9-939f-6a90d95621aa.png">
</p>

| BN                                                           | LN                                                           |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Batch 차원에서 정규화                                        | Feature 차원에서 정규화                                      |
| Batch 전체에서 계산이 이루어짐<br />각 Batch에서 동일하게 계산 | 각 특성에 대하여 따로 계산이 이루어지며<br />각 특성에 독립적으로 계산한다. <br />Batch 사이즈에 상관이 없고 RNN에 매우 좋은 성능을 보임<br /> 동일한 층의 뉴런 간 정규화 |



------------

#### 3. Instance normalization 

<p align="center">
   <img src="https://user-images.githubusercontent.com/26568793/58152364-5ccf8f80-7ca7-11e9-9dc4-e31127ca7739.png">
</p>

instance 단위로 normalization을 수행하는 것으로, 영상전체가 아닌 각 **영상의 채널 단위로** normalization을 사용한다. BN은 batch 및 공간 위치의 전체에서 ‘모든 이미지’를 정규화한다. 반대로 IN은 각 배치를 독립적으로, 즉 **공간 위치에서만 정규화를 진행**한다. style transfer을 위해 고안된 Instance Normalizaion은 network가 원본 이미지와 변형된 이미지가 구분할 수 없는 특성을 가지길 바라며 설계된 것으로, 이미지에 국한된 정규화로 RNN에는 사용할 수 없다. real-time-generation에 효과적이다. 

----------------------

#### 4. [Group normalization](<https://www.youtube.com/watch?v=m3TN9FFmqsI>)

<p align="center">
   <img src="https://user-images.githubusercontent.com/26568793/58160580-b5a82380-7cb9-11e9-99d2-5c6e533c4cf5.png">
</p>

각 채널을 N개의 Group으로 나누어 normalize 시켜주는 기술이다.  group normalization은 기존 normalization과 연관된 점이 많다. 

* **Layer Norm** :  Group Norm에서 N개의 채널을 나누어 Norm을 해줬다면 LN에서는 모든 채널을 하나의 그룹으로 넣어 Norm을 진행한다. 

* **Instance Norm** : Group Norm에서 N개의 채널을 나누어 Norm을 해줬다면 IN에서는 하나의 채널을 이용하여 Norm을 진행한다. 

* **Batch Norm** : IN과 비교했을 때 IN이라는 것은 batch당 하나의 이미지를 Norm하는 것이라 볼 수 있다. 



논문에서는 'Batch'라는 것이 언제나 이상적인 Normalization이 될 수 없음을 보여주며, 또한 Batch에 독립적으로 동작하는 기존의 LN과 IN보다 성능이 좋음을 실험을 통해 보여주었다. 더 자세한 내용은 위 연결된 링크에서 간단한 논문 리뷰를 볼 수 있다. 



# 기타 

####  1.  [Multi modal 분포]([https://learnai.tistory.com/category/Deep%20Learning](https://learnai.tistory.com/category/Deep Learning))

Mode가 여러 개 존재하는 것으로 mnist의 경우에는 각각의 숫자 10개가 mode에 해당하는 것이다. 

Mode라는 것은 최빈값으로 가장 빈도가 높은 값을 의미한다. 

#### 2.Mode collapse

D와 G가 서로를 속고 속이며 제자리를 맴도는 것, 즉 양쪽 모두 수렴할 수 없게 된다. G입장에서는 어떤 data를 생성하든 D만 속이면 되는 것이기 때문이다. 

