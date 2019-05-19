# Normalization 

### Batch normalization 

![1558265295017](C:\Users\Domirae\AppData\Roaming\Typora\typora-user-images\1558265295017.png)

각 층의 출력 값을 정규화하는 방법, DNN은 학습의 층이 깊어지면 깊어질수록 weight값의 변화가 가중돼서 쌓이면 값의 변화가 커지기 때문이다. 즉, hidden layer 내의 값의 변화가 큰 것이 문제가 되는 것이다. 처음 학습했던 노드의 분포와 다음에 학습하는 분포가 다르다면 학습이 잘 안된다. 이게 바로 ‘Internal Covariate Shift’이다. 이를 해결하기 위해서 Batch norm이 제안되었는데, BN은 신경망 안에 포함되어 training 시에 평균과 분산을 조정하는 과정 역시 같이 조정이 된다. 

**[다시 이해하기]** [^4]

![1558265438520](C:\Users\Domirae\AppData\Roaming\Typora\typora-user-images\1558265438520.png)

W1은 X를 바탕으로 학습, 복습, 업데이트 된다. X는 proprocessing되어 normalized input이므로 layer “A”는 비슷한 input distribution을 가진다. 따라서 안정적으로 W1을 학습할 수 있다. 

![1558266294132](C:\Users\Domirae\AppData\Roaming\Typora\typora-user-images\1558266294132.png)

W2는 a를 바탕으로 학습, 업데이트 된다. f(XW1)은 W1의 값에 따라 분포가 달라진다. 즉, layer B의 input distribution은 고정되어 있지않다. 따라서 안정적으로 W2를 학습할 수 없다.  

학습 과정에서 가중치의 값이 ![1558266325939](C:\Users\Domirae\AppData\Roaming\Typora\typora-user-images\1558266325939.png)로 업데이트되면 이전 레이어의 activation 또한  ![1558266330962](C:\Users\Domirae\AppData\Roaming\Typora\typora-user-images\1558266330962.png)로 바뀌게 된다. 은닉층의 입장에서는 인풋 값의 분포가 계속 널뛰는 것이나 마찬가지이다. 입력 분포의 형태가 유지되지 않으므로 학습도 잘 진행되지 않는다. 그라디언트 값이 큰 학습 초기일수록 문제가 더 심각해진다.

바로 해당 문제가 ‘Internal covariate shift’라고 한다. 말 그대로 입력층보다 깊은( internal에 있는) 입력값은 공변량(covariate)이 고정된 분포를 갖지 않고 이리저리 움직(shift)인다는 것이다. 

이를 해결하기 위해서 BN을 사용한다. 

해당 문제를 해결하기 위해서는 초반 입력데이터를 표준화 했던 것처럼 은닉층의 입력도 표준화한다면 안정적으로 깊은 레이어의 가중치 역시 학습시킬 수 있을 것이다. 즉 이는 이전 층의 출력(raw activation)을 표준화 한다는 의미와 같다.  

![1558266413531](C:\Users\Domirae\AppData\Roaming\Typora\typora-user-images\1558266413531.png)

딥러닝은 거의 항상 전체의 sample을 mini batch로 나누어 학습을 진행하여 가중치를 업데이트 하기 때문에 이전층의 출력을 표준화 할 때도 각 batch마다 따로 표준화를 진행하면 된다. 따라서 mini batch의 평균과 표준편차로 표준화한 activation 값을 은닉층 B의 입력으로 사용하면 은닉층B 역시 고정된 분포로 학습을 진행할 수 있다. 하지만 이런 식으로 은닉층의 입력을 표준화 하면 gradient update 과정에서 bias 값이 무시된다. 따라서 bias대신 평향의 역할을 할 파라미터가 추가되어야 한다. 또한 raw activation의 분포를 고정하는 것이 좋긴하지만 항상 N(0,1)로 고정할 필요는 없다. 적절하게 scaling, shifting된 activation을 사용하는 것이 학습에 도움이 될 수 있다. 따라서 해당 방식을 이용하여 activation function의 입력으로 사용하는 것을  BatchNorm이라고 한다. 

[^4]:  [https://de-novo.org/2018/05/28/batch-normalization-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0/](https://de-novo.org/2018/05/28/batch-normalization-이해하기/)

