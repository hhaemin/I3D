## Action Recognition with an Inflated 3D CNN
**Action Classification의 다양한 논문 분석 및 기능 구현**

**Video Recognition**
- 동영상 데이터는 기본적으로 공간적, 시간적 요소로 분해될 수 있다. 
- 공간적 부분은 동영상에서 묘사된 장면과 물체에 관한 정보를 담고있다. 
- 시간적 부분은 관찰자(카메라)와 물체의 움직임에 관한 정보를 담고 있다.

**논문 분석**
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

- ConvNet + LSTM은 high-level variation을 modeling할 수 있지만, low-level motion을 잡아내는데 어려움이 있다.
- 또한 다수의 frame은 역전파를 수행해야하기 때문에 high computation power 필요
- Two-Stream networks는 short temporal snapshots(비디오의 RGB 이미지 1개)와 외부에서 연산한 optical flow N개 쌓아서 averaging을 통해 classification을 수행
- Optical flow의 경우 horizental, vertical 2개의 채널로 구성되었기 때문에 conv layer 2개 사용
- test시에는 비디오로부터 multiple snapshots을 샘플링하고 예측값의 average값을 사용

4. 3D-Fused Two-Stream 

- Two-Stream network에서 마지막 conv layer 이후에 공간과 flow stream을 결정한 것
- time, x, y, dimensions가 3x3x3 3D conv layer (output 512 channels)
- 3x3x3 3D max-pooling layer, fc layer를 통과하는 형태
- two-stream/ 3D-fused two-stream 모두 end-to-end 방식으로 train

5. Inflating 2D ConvNets into 3D (I3D)

- 2D ConvNets을 3D ConvNet으로 convert하기 위해 매우 간단한 방법 사용
- temporal dimension을 추가하는 것으로 NxN필터를 NxNxN로 변경 -> 모든 필터와 pooling kernel에 적용

---
### Dataset

**UCF-101**
- 101개 동작 카테고리의 13320개 비디오를 통해 UCF101은 동작 측면에서 가장 큰 다양성을 제공하며 카메라 움직임, 개체 모양 및 포즈, 개체 크기, 관점, 어수선한 배경, 조명 조건 등의 큰 변화가 있는 가장 큰 다양성을 제공
- Human-Object Interaction
- Body-Motion Only
- Human-Human Interaction
- Playing Musical Instruments
- Sports
---
### 결론
- 논문 분석 결과 Two-Stream I3D을 이용한 모델이 가장 성능이 좋았다.
- 기본 오픈소스는 kinetics-400을 이용하였지만 Kinetics-600으로 바꿔주었다.
- 프레임이 50일때보다 프레임 200일 때 더 정확한 결과를 도출하는 것을 알 수 있었다.
- 영상의 각도, 위치에 따라서 다른 값을 도출해내는 것을 확인할 수 있었다.

- 아직 정확한 예측값을 내기에는 부족한 모델이라고 생각이 들지만, 계속해서 성장하는 딥러닝 분야에서 너무 오래된 모델을 이용해서 프로젝트를 진행한 탓도 있는 것 같다.
