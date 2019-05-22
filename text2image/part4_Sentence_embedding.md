# Sentence Embedding

기존 임베딩 방식을 이용하면 

Red rose with yellow stem = ⅕ * (1, 1, 1, 1, 1) = (0.2, 0.2, 0.2, 0.2, 0.2)

yellow rose with red stem = ⅕ * (1, 1, 1, 1, 1) = (0.2, 0.2, 0.2, 0.2, 0.2)

보는 것과 같이 서로 다른 mode이지만 충돌이 불가피한 문제가 발생한다.  따라서 **순서 정보**를 담고 있는 embedding의 필요성이 대두되었다. 기존 모델들은 대부분 Glove나 Word2Vec을 더하거나 평균을 내어 sentence embedding으로 활용하였으나 근본적으로 한계가 있다. 따라서 순서 정보를 포함하는 임베딩의 필요성을 느끼게 되었고 해당 문제를 USE( Universal-sentence-encoder)을 이용하여 해결하고자 한다.

----------

### USE의 간단한 구조 

Universal sentence Encoder는 텍스트 분류, 의미론적 유사성, 클러스터링 및 기타 자연어 처리에 사용할 수 있는 고차원 벡터를 이용하여 텍스트를 인용하는 방법이다. 

해당 모델은 문장, 구 또는 짧은 단란과 같이 단어 길이가 더 긴 텍스트에 대해 학습되며 최적화된 방식이다. 또한 단순히 단어가 아닌 시퀀스의 의미를 모델링하였음. Transformer와 Deep Average Network를 SNLI에 학습시킨 모델이다.

* Transformer Encoder?

  이를 이해하기 위해서 **attention is all you need 논문**을 리딩했음.  attention의 시초는 기존의 시퀀스 모델을 보면 cnn이나 rnn을 encoder, decoder로써 활용한다. 주로 recurrent model은 long-term-dependency 문제가 발생하는데, 해당 문제는 어떤 정보와 다른 정보 사이의 거리가 멀 때 해당 정보를 이용하지 못한다는 것이다. 그러나 attention is all you need 논문에서는 이를 배제하고 attention 매커니즘을 이용하여 encoder와 decoder를 제작한다. 해당 network를 transformer라 한다. 이를 통해 paralleizable이 가능해 진다고 한다. 이를 통해 입출력의 global dependency를 잡아낸다. 

