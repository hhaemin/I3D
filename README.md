## Action Recognition with an Inflated 3D CNN
**Action Classification의 다양한 논문 분석 및 기능 구현**

Video Recognition
- 동영상 데이터는 기본적으로 공간적, 시간적 요소로 분해될 수 있다. 
- 공간적 부분은 동영상에서 묘사된 장면과 물체에 관한 정보를 담고있다. 
- 시간적 부분은 관찰자(카메라)와 물체의 움직임에 관한 정보를 담고 있다.

논문 분석
1. 2D ConvNet + LSTM

- 각 frame별로 features를 extract하고 비디오 전체에 대해 예측을 실시하는 image classification의 개념을 그대로 적용한 방식
- Bag of words image modeling 방식 접근 -> temporal structure(시간)를 나타내는데 어려움이 있다.
- LSTM과 같은 recurrent layer을 적용하여 temporal ordering, 넓은범위에 대한 의존성을 잡아낼 수 있다.

- LSTM(RNN의 한 종류) : 긴 의존기간을 필요로 하는 학습을 수행할 능력을 갖고있다.
- 긴 의존기간의 문제를 피하기 위해 명시적으로 설계됨

2. 3D ConvNet

- 기존의 convolutional networks에 spatio-temporal filter를 추가함으로써 시공간데이터를 계층적으로 나타낼 수 있다.
- 하지만 2D Conv와 비교하면 파라미터 수가 급격히 늘어나기 때문에 train을 시키는데 무리가 있다.
- architecture C3D : 파라미터 수를 줄이기 위해서 BN을 매 conv,fc layer이후에 수행
- 첫 번째 pooling layer에서도 temporal stride를 1에서 2로 증가하여 사용

3. Two-Stream

- 두 개의 스트림으로 나눕니다. 
- 각 스트림은 deep ConvNet을 사용하여 구현되며, softmax 점수는 late fusion으로 결합된다.
- 누적된 L2 정규화 softmax 점수에 대한 다중 클래스 선형 SVM을 기능으로 평균화하고 훈련한다.

4. 3D-Fused Two-Stream 




- 논문 분석 결과 Two-Stream 3D ConvNet을 이용한 모델이 가장 성능이 좋았다.
- 기본 오픈소스는 kinetics-400을 이용하였지만 Kinetics-600으로 바꿔주었다.
- 
