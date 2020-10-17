# mit-bih arrhythmia superclass


#### 추가로 수정 및 고려할 사항 (201017 update)
* 데이터 normalization 후 model 학습을 시키는 방법으로 해야 계산하는 양 감소 가능.

* CNN 하면서 왜곡(distortion)이 발생 함. 
-> conv, pooling, stride 등을 이를 고려해서 적절하게 적용하여 기존의 데이터에서 크게 벗어나지 않게 해야 함.

* 성능 측정 시 accuracy 말고 f1 score, recall, precision을 표시해서 훈련이 정확하게 되었는지를 확인해야 함.

* 클래스의 개수가 편향되어 있으면 학습이 치우쳐져서 될 수 있다.
그러면 5000개로 동일하게 나누면 학습이 공평하게 되는가?
-> 그럴 수도 있지만 아닐 수도 있다. 확신할 수 없다.

* filter 여러 개로 conv 연산하여 조합하는 방법도 있다(단점 : 계산 양이 늘어남)

* class 수 많아지면 filter/depth 중 하나를 증가시켜야 함 -> ?
* window size 설정 시 이유 필요 -> 점진적으로 낮추는 방법, 또 다른 방법 

* code를 따라가면서 왜 이렇게 사용되었는지 한 줄씩 검토.

### 찾아보기, 적용 필요하다 생각되면 수정
inception
residual 기법.
1D CNN 을 왜 쓰는지, 어떻게 사용하는지 조사.
