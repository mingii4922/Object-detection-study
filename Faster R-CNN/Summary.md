# Faster R-CNN
### Faster R-CNN: Towards Real-Time Object Detection with Region Proposal Networks, NIPS, 2016

---
### Abstract
Fast R-CNN을 보완한 논문으로 2-stage 기반 object detection의 근본

SPPnet과 Fast R-CNN은 region proposal 계산에서 bottleneck 현상을 지적함.

이를 보완하기 위해 region proposal network(RPN)을 제안하며, end-to-end framework를 제안

---
### Contribution

* Fast R-CNN의 단점인 region proposal을 개선하기 위해 selective search 대신 Region Proposal network(RPN)을 제안

---
### 사전지식

selective search:

fine tuning

weight sharing

---
### Region proposal network(RPN)



---
### training

1. ImageNet으로 pretrained model을 통해 region proposal을 위해 end-to-end로 RPN을 학습
2. 1단계에서 학습시킨 RPN에서 기본 CNN을 제외한 Region proposal layer만 가져와 Fast R-CNN와 동일하게 학습
3. 학습시킨 Fast R-CNN의 weight를 고정하고 RPN에 해당하는 layer만 fine tuning
4. weight sharing된 conv layer를 고정하고 Fast R-CNN의 layer만 fine tuning

---
