## Action Recognition with an Inflated 3D CNN
**Action Classification의 다양한 논문 분석 및 기능 구현**

Video Recognition
- 동영상 데이터는 기본적으로 공간적, 시간적 요소로 분해될 수 있다. 
- 공간적 부분은 동영상에서 묘사된 장면과 물체에 관한 정보를 담고있다. 
- 시간적 부분은 관찰자(카메라)와 물체의 움직임에 관한 정보를 담고 있다.

논문 분석




- 논문 분석 결과 Two-Stream 3D ConvNet을 이용한 모델이 가장 성능이 좋았다.
- 기본 오픈소스는 kinetics-400을 이용하였지만 Kinetics-600으로 바꿔주었다.
- 
