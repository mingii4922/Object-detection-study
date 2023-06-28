# Fast R-CNN
### Fast R-CNN, CVPR, 2015
---
### Abstract
* R-CNN보다 단어 그대로 fast하며, 성능도 향상시킨 논문

---
### Contribution
* Multi-task learning 적용
* Single-stage: object 분류와 공간위치를 세분화 하는 방법을 동시에 학습
* SVM 대신 Softmax 활용
* end-to-end 방식의 back-propagation이 한번에 가능
---
### 사전지식
* RoI(region of interest): 관심영역을 뜻함
* Selective Search: segmentation 분야에 많이 쓰이며, object와 주변간의 색감(color), 질감(texture) 등을 파악하여 인접한영역끼리 유사도를 측정해 유사한 pixel끼리 합쳐나가는 방식
* Anchor Box: 사전에 정의된 고정된 크기의 box
* Multi-task learning: 하나의 모델을 활용해 여러개의 task를 동시에 처리하는 machine learning 방법
    * Hard sharing: 뿌리(CNN)가 동일하게 시작되어 각각의 representation을 학습
    * Soft sharing: 서로 다른 뿌리(CNN)에서 시작하여 중간중간 정보를 공유함 -> 모델이 더 큰 느낌
    * -> 여러 task가 서로 어느정도의 상관관계가 존재한다면 전체적인 성능이 높아짐

* Spatial pyramid pooling networks(SPPnets): 전체 input image에 대한 convolution feature map을 계산하고, shared feature map에서 추출한 feature vector를 사용하여 object detection task를 해결

---
<img src="https://github.com/mingii4922/object-detection/assets/79297596/c47bbdb7-afde-4b1a-94d9-792fbed69789" width="600" height="300">

R-CNN(2014)은 **2000장**의 region proposals를 CNN model의 입력으로 받아 독립적인 학습방식을 거침 
>학습이 오래걸림

Fast R-CNN은 object로 의심되는 영역을 뽑을 때 selective search 대신 pre-trained CNN을 **input에 바로 활용**

  > 이는 추출된 feature에서 size를 warp시킬 필요없이 RoI(Region of Interest) pooling을 통해 고정된 크기의 feature vector를 fc layer에 전달함.

---
### RoI pooling layer

<img src="https://github.com/mingii4922/object-detection/assets/79297596/0353fa00-50a4-4575-add6-70030e4a7707" width="600" height="300">

* max pooling을 통해 Conv feature map을 고정된 크기 $(H \times W)$로 변경
  > 각 검정색 박스 영역에서 가장 큰 값 하나씩 가져옴
  > -> **RoI의 크기를 input에 맞게 warping 해주는 느낌**
  > maxpooling으로 인해 어쩔 수 없이 정확한 bounding box 측정이 한계적
  
backbone network는 VGG-16

---
### Loss function

* multi-task learning을 진행하기 때문에 bounding box 위치 측정(regression)과 object 탐지(classfication)을 동시에 진행

$$L(p,u,t^u,v) = L_{cls}(p,u) + \lambda[u \geq 1] \, \, \, L_{loc}(t^u,v),$$

>$p=(p_0,...,p_K)$는 $K+1$개 만큼의 class(object 종류)로 각 RoI의 softmax 값 (1=배경)
$u$는 ground-truth class
$t^k=(t_x^k,t_y^k,t_w^k,t_h^k)$는 class $K$에 대해 추정한 bounding box 위치,너비,높이
$v$는 실제 bounding box 값

---

 
   
---
* R-CNN은 selective search로 region proposals로 2000개를 추출하기 때문에 많은 시간이 걸린다는 단점이 존재
  * 이를 보완하기 위해 SPPNet(Spatial pyramid pooling) 논문을 기반으로 RoI pooling을 활용
---
추가적으로 multi-task loss를 활용해 모델을 개별적으로 학습시킬 필요가 없음

* 이건 Faster R-CNN: RPN(region proposal network): object detection task에서 중요한 작업으로 추출된 feature map을 input으로 활용하는 network를 뜻함.
  * input -> bounding box -> sort -> NMS
  * 선택한 bounding box와 anchor box를 비교하여 확률이 가장 높은 object에 대해서 sort를 진행하고 NMS 결과를 수행
---
Fast R-CNN은 R-CNN과 달리 feature map을 추출할 때 CNN 한번만 거
* 장점: R-CNN(two-stage)보다 빠르고 성능도 향상시킴
* 단점: real-time에 적용이 어려움
> 이후 Faster R-CNN에서는 region proposal에서 병목현상이 발생하기 때문이라고 판단함

---
