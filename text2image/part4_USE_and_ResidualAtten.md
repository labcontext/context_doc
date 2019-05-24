# USE와 Residual-Attention을 이용한 text2Image GAN 

이번 문서는 part3에서 언급한 문제를 해결하기 위해 진행했던 방식을 설명하는 문서입니다.

1. Sentence Embeding -->USE 
2. Residual Attention 

[코드 확인하러가기](<https://github.com/labcontext/text-to-image-with-residual-attention-and-USE>)

[발표자료 보러가기](https://drive.google.com/file/d/1sDPv8FR69uYGGTD-gIF28l19hoy-FakQ/view?usp=sharing)

## 1.Sentence Embedding

기존 임베딩 방식을 이용하면 

Red rose with yellow stem = ⅕ * (1, 1, 1, 1, 1) = (0.2, 0.2, 0.2, 0.2, 0.2)

yellow rose with red stem = ⅕ * (1, 1, 1, 1, 1) = (0.2, 0.2, 0.2, 0.2, 0.2)

보는 것과 같이 서로 다른 mode이지만 충돌이 불가피한 문제가 발생한다.  따라서 **순서 정보**를 담고 있는 embedding의 필요성이 대두되었다. 기존 모델들은 대부분 Glove나 Word2Vec을 더하거나 평균을 내어 sentence embedding으로 활용하였으나 근본적으로 한계가 있다. 따라서 순서 정보를 포함하는 임베딩의 필요성을 느끼게 되었고 해당 문제를 USE( Universal-sentence-encoder)을 이용하여 해결하고자 한다.

----------

### [USE가 뭐지..?](<https://www.dlology.com/blog/keras-meets-universal-sentence-encoder-transfer-learning-for-text-data/>)

Universal sentence Encoder는 텍스트 분류, 의미론적 유사성, 클러스터링 및 기타 자연어 처리에 사용할 수 있는 고차원 벡터를 이용하여 텍스트를 인용하는 방법이다. 

해당 모델은 문장, 구 또는 짧은 단락과 같이 단어 길이가 더 긴 텍스트에 대해 학습되며 최적화된 방식이다. 또한 단순히 단어가 아닌 시퀀스의 의미를 모델링하였다. 
뚜렷한 설계 목표를 달성하기 위해 서로 다른 encoder 아키텍쳐를 가진  Transformer Encoder와 DAN(Deep Average Network)을 사용한다.  

<p align="center">
   <img src="https://user-images.githubusercontent.com/26568793/58266931-9a264100-7dbd-11e9-9ffa-46fe1325cade.png">
</p>

* **Transformer Encoder**는 더 큰 모델의 complexity와 자원의 consumption의 비용으로 높은 정확성을 목표로 한다. 
* **DAN Encoder**은 Deep Averaging Network(DAN)을 이용하여 정확도가 약간 감소된 효율적인 추론을 목표로 한다. 

