# Transformer 

part4에 등장하는 USE의  Transformer Encoder을 이해하기 위한 사전 지식 문서



이를 이해하기 위해서 **attention is all you need 논문**을 리딩했다.  attention의 시초는 기존의 시퀀스 모델을 보면 cnn이나 rnn을 encoder, decoder로써 활용한다. 주로 recurrent model은 long-term-dependency 문제가 발생하는데, 해당 문제는 어떤 정보와 다른 정보 사이의 거리가 멀 때 해당 정보를 이용하지 못한다는 것이다. 그러나 attention is all you need 논문에서는 이를 배제하고 attention 매커니즘을 이용하여 encoder와 decoder를 제작한다. 해당 network를 transformer라 한다. 이를 통해 paralleizable이 가능해 진다고 한다. 이를 통해 입출력의 global dependency를 잡아낸다. 

