# R-CNN
> Rich feature hierarchies for accurate object detection and semantic segmentation, CVPR, 2014
---
> object detection 분야에서 딥러닝 방식 중 하나
>> 2-stage detector: 1.문제의 위치를 찾고, 2. 분류 문제를 순차적으로 해결

>> 1-stage detector: 두 문제(위치+분류)를 동시에 수행
---
![object detection](https://github.com/mingii4922/object-detection/assets/79297596/9fdca7c8-5674-40b4-8a3c-e519cd22617e)

---
classification: object의 클래스를 분류
classification + localization: signle object에 대해 object의 위치를 bounding box로 찾고, 클래스를 분류
object detection: multiple objects의 상황에서 각 object에 대해 위와 같은 작업 수행
instance segmentation: object의 위치를 edge로 찾는 것: 각 object는 서로 다른 색(class)으로 구분
semantic segmentation: instance segmentation과 달리, 동일한 class를 가진 object는 같은 색으로 분할

---
![R-cnn1](https://github.com/mingii4922/object-detection/assets/79297596/b1c48a7f-3201-4726-ab9f-4ab5c9355b86)
+ 해당 논문은 2-stage detector로 구성됨
  
1. Region proposal: image에 object가 있을 법한 위치(bounding box)를 추출
2. CNN: pre-trained 된 CNN model의 feature map을 활용
3. SVM(Support Vector Machine): 선형 지도학습모델을 통해 분류
4. Bounding Box Regression: regressor를 통해 bounding box 위치좌표 찾기

---
1. Region proposal
  Selective Search 알고리즘을 이용해 임의의 bounding box를 설정
  Selective Search: segmentation 분야에 많이 쓰이며, object와 주변간의 색감(color), 질감(texture) 등을 파악하여 인접한영역끼리 유사도를 측정해 유사한 pixel끼리 합쳐나가는 방식
  sliding window는 너무 많은 영역에 대해 연산을 거치기 때문에 느리다는 단점이 존재

----
+ 장점: CNN을 이용해 각 region의 class를 분류할 수 있음.
+ 단점: 전체 framework를 end-to-end 방식으로 학습할 수 없음. -> **global optimal solution을 찾기 어려움.**
