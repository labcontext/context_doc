# Transformer 

part4에 등장하는 USE의  Transformer Encoder을 이해하기 위한 사전 지식 문서

**Transformer**라는 개념을 이해하기 위해서 [**attention is all you need 논문**](https://arxiv.org/abs/1706.03762)을 리딩했다. 

해당 논문에서는 Recurrent, Convloution을 사용하지 않고 Attention만을 사용한 간단한 신경망 구조를 통해 기계 번역 분야에서 SOTA를 얻은 방식을 설명한다. 

----------------

### 1. RNN과 LSTM의 한계 

앞으로 언급할 문제는 두가지이다. 

> 1. Paralleization의 한계
> 2. long-term-dependency 문제

RNN모델은 input과 output의 시퀀스의 위치들을 계산하는 것에 탁월하다.  이전 정보와 현재 input 값을 이용하여 새로운 hidden state를 만들어낸다. 그렇기 때문에 RNN은 순차적인 특성을 가지고 있다. 그렇기 때문에 paralleizable한 속성이 부족하다고 한다. 즉, RNN과 LSTM은 시퀀스의 원소를 순회하면서 지금까지 처리한 정보를 state에 저장하는 형태이기 때문에 시퀀스 길이가 길어지면 길어질 수 록 batch로써 풀고자할 때 문제가 된다. ( 연산을 병렬적으로 사용하기 힘들다는 말이다.  왜냐하면 순차적인 정보를 이용하여 학습을 진행하기 때문이다.)

또한 주로 recurrent model은 long-term-dependency 문제가 발생하는데, 해당 문제는 어떤 정보와 다른 정보 사이의 거리가 멀 때 해당 정보를 이용하지 못한다는 것이다. 그러나 attention is all you need 논문에서는 이를 배제하고 attention 매커니즘을 이용하여 encoder와 decoder를 제작한다. 

### 2. Transformer과 Attention

<p align="center">그래서 Transformer가 뭔데?</p>

<p align="center">
   <img src="https://user-images.githubusercontent.com/26568793/58311386-d1d7cc00-7e43-11e9-832d-d9240243c054.png">
</p>

위 사진처럼 encoder-decoder 구조를 가지고 있는 network를 transformer라 한다. 

USE에서는 encoder만을 채택하여 사용하므로 encoder에 관한 설명만 하고 넘어가도록 하겠다. 더 자세한 설명은 아래 첨부한 링크의 페이지에서 더 좋은 정보를 얻을 수 있다. 



### 3. Transformer의 효과

**attention mechanism**은 앞서 언급한 RNN의 문제와는 달리 data의 시퀀스 길이 의존도가 낮다. 따라서 attention mechanism을 이용하는 Transfomer 모델은 input과 output의 global dependency를 잡아내며, 병렬처리를 수월하게 진행하게 해준다. 

순차적인 연산을 줄이고자 하는 목표로 ByteNet등이 생겨났는데, 이들은 모두 hidden representation을 parallel하게 계산하고자, CNN을 활용한다. 그러나 이들은 2개의 positoin 상 멀리 떨어져있는 input - output을 연결하는데에 많은 연산을 필요로 한다(number of operation required). 따라서 distant position에 있는 dependency를 학습하기에는 힘들다. Transformer에서는 attention-weighted position을 평균을 취해줌으로써 effective는 잃었지만, 이 operation이 상수로 고정되어있다. effective에 대해선 Multi-Head Attention으로 이를 극복한다.(section 3.2)

Self-attention은 seq representation을 얻고자 한 sequence에 있는 다른 position을 연결해주는 attention기법이다. 이는 지문이해나 요약등의 과제에서 다양하게 활용되고 있다.

-----------

------------------------------

**해당 문서는 논문과 함께 [링크1]([https://github.com/YBIGTA/DeepNLP-Study/wiki/Attention-Is-All-You-Need-%EB%85%BC%EB%AC%B8%EB%A6%AC%EB%B7%B0](https://github.com/YBIGTA/DeepNLP-Study/wiki/Attention-Is-All-You-Need-논문리뷰)) , [링크2](<http://blog.naver.com/PostView.nhn?blogId=hist0134&logNo=221035988217&redirect=Dlog&widgetTypeCall=true>) 를 많이 참고하여 작성했습니다.** 