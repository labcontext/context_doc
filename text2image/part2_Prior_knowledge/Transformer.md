# Transformer 

part4에 등장하는 USE의  Transformer Encoder을 이해하기 위한 사전 지식 문서

**Transformer**라는 개념을 이해하기 위해서 [**attention is all you need 논문**](https://arxiv.org/abs/1706.03762)을 리딩했다.  



#### RNN과 LSTM의 한계 

앞으로 언급할 문제는 두가지이다. 

> 1. Paralleization의 한계
> 2. long-term-dependency 문제

RNN모델은 input과 output의 시퀀스의 위치들을 계산하는 것에 탁월하다.  이전 정보와 현재 input 값을 이용하여 새로운 hidden state를 만들어낸다. 그렇기 때문에 RNN은 순차적인 특성을 가지고 있다. 그렇기 때문에 paralleizable한 속성이 부족하다고 한다. 즉, RNN과 LSTM은 시퀀스의 원소를 순회하면서 지금까지 처리한 정보를 state에 저장하는 형태이기 때문에 시퀀스 길이가 길어지면 길어질 수 록 batch로써 풀고자할 때 문제가 된다. ( 연산을 병렬적으로 사용하기 힘들다는 말이다.  왜냐하면 순차적인 정보를 이용하여 학습을 진행하기 때문이다.)

또한 주로 recurrent model은 long-term-dependency 문제가 발생하는데, 해당 문제는 어떤 정보와 다른 정보 사이의 거리가 멀 때 해당 정보를 이용하지 못한다는 것이다. 그러나 attention is all you need 논문에서는 이를 배제하고 attention 매커니즘을 이용하여 encoder와 decoder를 제작한다. 

#### Attention mechanism의 효과





<p align="center">
   <img src="https://user-images.githubusercontent.com/26568793/58311386-d1d7cc00-7e43-11e9-832d-d9240243c054.png">
</p>

위 사진처럼 encoder-decoder 구조를 가지고 있는 network를 transformer라 한다. 