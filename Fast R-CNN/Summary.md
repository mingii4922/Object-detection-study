# Fast R-CNN
> Fast R-CNN, CVPR, 2015
---
* R-CNN보다 단어 그대로 fast하며, 성능도 향상시킨 논문
* single-stage로 이루어짐.

---
<img src="https://github.com/mingii4922/object-detection/assets/79297596/c47bbdb7-afde-4b1a-94d9-792fbed69789" width="600" height="300">

> R-CNN model은 2000장의 region proposals를 CNN model의 입력으로 받아 독립적인 학습방식을 거침 -> 이는 학습이 오래걸리는 단점이 존재
>
> Fast R-CNN은 object로 의심되는 영역을 뽑을 때 selective search 대신 pre-trained CNN을 활용
>
> 이는 추출된 feature에서 size를 warp시킬 필요없이 RoI(Region of Interest) pooling을 통해 고정된 크기의 feature vector를 fc layer에 전달함.
>
> 추가적으로 multi-task loss를 활용해 모델을 개별적으로 학습시킬 필요가 없음

---
* multi-task loss:

---

---
