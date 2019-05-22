# 이전 연구 

2기에서 진행한 내용은 다음과 같다. mscoco dataset(caption과 대응하는 이미지)를 이용하여 학습을 진행한다. <https://github.com/chen0040/keras-text-to-image> 해당 코드를 기반으로 테스트를 진행.



### 첫번째 실험 

DCGAN / BatchNormalization / SGD 이용하여 모델링 --> 학습이 진행되는 과정에서 mode collapse발생.

### 두번째 실험

더 딥한 DCGAN 이용 / batchnormaliztion / SGD 이용 --> mode collapse가 더 빨리 찾아왔음 

아마도 일반적 GAN설계 마지막 activation function에서는 tanh로 사용, 최초 모델에서는 모델이 얕고 필터가 컸기 때문에 마지막 레이어를 제외한 레이어의 activation function을 tanh로 써도 학습에 무리가 없었다. 그러나 모델이 깊어짐에 따라 Vanishing gradient problem이 발생한 것으로 보인다. 

### 세번째 실험 

DCGAN을 기반으로 image to image translation분야에서 시도되는 여러 스킬을 조합해보았다.

- Batch Normalization --> Instance Normalization
- SGD --> Adam 으로 변경.

Instance Normalization으로 변경한 이유는 cross domain GAN과 관련된 시도에서 Batch Normalizaiotn으로 들어오는 노이즈가 GAN학습에 어떠한 영향을 끼칠지 장담하지 못하는 분위기인 것으로 보여 그 대용으로 사용한 것이며 전반적인 개선이 있었음. 

### 네번째 실험

세번째 모델을 토대로 image size와 resolution을 키우는 GAN 또는 VAE를 스택하려는 것을 시도 하려함. 이는 컴퓨팅 파워문제로 output feature를 64*64로하며 이를 다시 input으로 받아 사이즈를 키우는 모델을 stack하여 stackGan형태로 만드려고 한다. 

### 다섯번째 실험 

CVPRFLOWER 데이터를 활용하여 꽃사진과 꽃설명을 가지고 학습을 진행해보았다. SNGAN을 이용하여 설계하였으며 D부분을 SNGAN형태로, G부분을 deep convolutional하게 설계하였다. (SNGAN을 이용한 이유는 GAN역사에 따라서 실험을 진행해본 것) 

기존 시도와는 다르게 G에서 앞 단 convolution 레이어에 입력하기 전에 한번 Normalizaion을 해주고 매 convolution 레이어 이후에 넣었던 Normalization 부분을 여러 실험을 통해 빼 보았으며, activation 부분을 tanh로 바꾸어 학습을 진행하였다. 해당 실험에서 발견한 문제는 아래 문항에서 설명하도록 하겠다.

