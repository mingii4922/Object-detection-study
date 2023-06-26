# R-CNN
### Rich feature hierarchies for accurate object detection and semantic segmentation, CVPR, 2014
---
> object detection 분야에서 딥러닝 방식 중 하나
>> 2-stage detector: 1.문제의 위치를 찾고, 2. 분류 문제를 순차적으로 해결
>> 
>> 1-stage detector: 두 문제(위치+분류)를 동시에 수행
---
![object detection](https://github.com/mingii4922/object-detection/assets/79297596/9fdca7c8-5674-40b4-8a3c-e519cd22617e)

---
* classification: object의 클래스를 분류

* classification + localization: signle object에 대해 object의 위치를 bounding box로 찾고, 클래스를 분류

* object detection: multiple objects의 상황에서 각 object에 대해 위와 같은 작업 수행

* instance segmentation: object의 위치를 edge로 찾는 것: 각 object는 서로 다른 색(class)으로 구분

* semantic segmentation: instance segmentation과 달리, 동일한 class를 가진 object는 같은 색으로 분할

---
<center> <img src="https://github.com/mingii4922/object-detection/assets/79297596/09c675ab-921a-4e14-920c-5ea171c24760" width="800" height="300"> </center> 

+ 해당 논문은 2-stage detector로 구성됨
  
1. Region proposal: image에 object가 있을 법한 위치(bounding box)를 추출
2. CNN: pre-trained 된 CNN model의 feature map을 활용
3. SVM(Support Vector Machine): 선형 지도학습모델을 통해 분류
4. Bounding Box Regression: regressor를 통해 bounding box 위치좌표 찾기

---
1. Region proposal

* Selective Search 알고리즘을 이용해 임의의 bounding box를 설정
*  Selective Search: segmentation 분야에 많이 쓰이며, object와 주변간의 색감(color), 질감(texture) 등을 파악하여 인접한영역끼리 유사도를 측정해 유사한 pixel끼리 합쳐나가는 방식
*  sliding window는 너무 많은 영역에 대해 연산을 거치기 때문에 느리다는 단점이 존재
*  해당 연산을 거친 ROI 영역을 CNN의 입력 값에 넣어줌 -> 총 2000개의 ROI feature map

---
2. CNN

*  AlexNet 사용: (ImageNet)으로 pre-train
*  selective search를 통해 추출된 ROI영역을 227x227 사이즈로 고정시키고 마지막에 fc layer를 200,20으로 바꿈

---
3. SVM

*  machine learning에서 패턴 인식이나 자료 분석을 위해 사용되는 모델로, 분류 or 회귀에 사용
*  저자는 Softmax를 사용하지 않은 이유가 Appendix B 부분에서 mAP(mean average precision)가 SVM이 더 좋았다고 함
*  CNN을 통해 저차원으로 mapping된 feature vector들의 점수를 class별로 매기고, object인지 판별 => 즉, classifier역할

---
4. Bounding Box Regression

*  part 1.에서 사용된 selective search가 만든 bounding box는 정확한 위치좌표를 알지 못하기에 object를 정확히 찾는 역할을 추가함
*  실제 label(y)와 CNN을 통해 나온 bounding box 간의 차이를 줄이는 역할

$$ \begin{align}
\hat{G_{x}} &= P_{w} d_{x} (P) + P_{x}\\
\hat{G_{y}} &= P_{h} d_{y} (P) + P_{y}\\
\hat{G_{w}} &= P_{w} exp(d_{w} (P))\\
\hat{G_{h}} &= P_{h} exp(d_{h} (P))\\
\end{align}
$$

> G:ground truth, P: predict
>
> x,y: box 중심 좌표
>
> w,h: box 너비, 높이
>

----
+ 장점: CNN을 이용해 각 region의 class를 분류할 수 있음.
+ 단점: 전체 framework를 end-to-end 방식으로 학습할 수 없음. -> **global optimal solution을 찾기 어려움.**
+ 단점: 시간이 오래 걸림 -> real time 분석이 어려움
