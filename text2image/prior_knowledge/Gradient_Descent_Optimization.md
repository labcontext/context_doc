#  Gradient Descent Optimization Algorithms 
![image](https://user-images.githubusercontent.com/26568793/57982028-60ca9a00-7a7a-11e9-8f17-2611c1756718.png)

[참고링크](https://www.slideshare.net/yongho/ss-79607172)

Neural network의 weight를 조절하는 과정에서 사용하는 방법으로, 네트워크에서 내놓는 결과값과 실제 결과값 사이의 차이를 정의하는 함수인 Loss function의 값을 최소화하기 위해서 기울기 값을 사용하는 방법이며 gradient의 반대 방향으로 일정크기만큼 이동해내는 것을 반복하여 loss function의 값을 최소화하는 값을 찾는 것. 이 때 Loss function을 계산할 때 전체 train set을 사용하는 것을 Batch Gradient Descent라고 한다. 여러 방법들이 존재하지만 실험에 사용한 Optimizer에 대한 간단한 소개만 하도록 하겠다. 



------

### SGD(Stochastic  Gradient Descent)

![image](https://user-images.githubusercontent.com/26568793/57982033-72ac3d00-7a7a-11e9-8787-87d6d015ce45.png)

이 방법에서는 loss function을 계산할 때 전체 데이터(batch) 대신 일부 조그마한 데이터의 모음(mini-batch)에 대해서만 loss function을 계산한다. 이 방법은 batch gradient descent 보다 다소 부정확할 수는 있지만, 훨씬 계산 속도가 빠르기 때문에 같은 시간에 더 많은 step을 갈 수 있으며 여러 번 반복할 경우 보통 batch의 결과와 유사한 결과로 수렴한다. 또한, SGD를 사용할 경우 Batch Gradient Descent에서 빠질 local minimum에 빠지지 않고 더 좋은 방향으로 수렴할 가능성도 있다.





------

### [Adam](https://dalpo0814.tistory.com/29)

'잘 모르겠으면 그냥 Adam을 써라'에서 자주 듣는 Adam이다. 

Adagrad와 RMSProp의 장점을 섞어 놓은 것으로 step size가 gradient의 rescaling에 영향을 받지 않는다는 것이다. 해당 개념은 RMSProp에 모멘텀의 개념을 포함한 것이고, 또한 Adam optimizer의 강점은 bounded step size로 예측 가능한 step size로 인해 하이퍼파라미터 설정 시 step size를 미리 적절한 값으로 세팅할 수 있다는 점이다. 

[^3]: <https://dalpo0814.tistory.com/29>
