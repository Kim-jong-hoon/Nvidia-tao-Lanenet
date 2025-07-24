# Nvidia-tao-Lanenet
## "Lane net" 이란?
--> Nvidia TAO Toolkit에서 제공하는 차선 인식 모델, 자율주행 차량 등에 매우 유용하다.
# LaneNet: NVIDIA TAO Lane Detection Model

LaneNet은 NVIDIA TAO Toolkit 기반의 **차선 검출 모델(Lane Detection)** 입니다. 이 모델은 도로 이미지에서 차선을 픽셀 단위로 분리하여, 자율주행, ADAS 등 다양한 운전자 지원 시스템에서 활용할 수 있도록 설계되었습니다.

---

## 목차

- [특징](#%ED%8A%B9%EC%A7%95)
- [개요](#%EA%B0%9C%EC%9A%94)
- [TAO Toolkit과의 통합](#tao-toolkit%EC%99%80%EC%9D%98-%ED%86%B5%ED%95%A9)
- [설치 및 준비사항](#%EC%84%A4%EC%B9%98-%EB%B0%8F-%EC%A4%80%EB%B9%84%EC%82%AC%ED%95%AD)
- [훈련 및 전이 학습](#%ED%9B%88%EB%A0%A8-%EB%B0%8F-%EC%A0%84%EC%9D%B4-%ED%95%99%EC%8A%B5)
- [추론 및 배포](#%EC%B6%94%EB%A1%A0-%EB%B0%8F-%EB%B0%B0%ED%8F%AC)
- [사용 예시](#%EC%82%AC%EC%9A%A9-%EC%98%88%EC%8B%9C)
- [참고 링크](#%EC%B0%B8%EA%B3%A0-%EB%A7%81%ED%81%AC)

---

## 특징

- **Semantic Segmentation 기반 차선 검출**: 도로 이미지에서 각 차선을 픽셀 단위로 분리합니다.
- **실시간 추론**: TensorRT 최적화를 통해 NVIDIA Jetson 플랫폼 등에서 실시간 추론이 가능합니다.
- **전이 학습 지원**: 기존에 학습된 모델에 사용자 도메인 데이터를 추가하여 손쉽게 모델을 튜닝할 수 있습니다.
- **다중 차선 검출**: 복수의 차선을 동시에 검출할 수 있습니다.
- **NVIDIA TAO Toolkit 통합**: TAO의 CLI 또는 Jupyter Notebook 환경을 통해 모델 훈련, 평가, 최적화가 가능합니다.

---

## 개요

LaneNet은 딥러닝 기반의 차선 검출 모델로서, 도로 이미지 내 차선을 분리하는 segmentation 작업을 수행합니다. 주요 활용 분야는 자율주행 시스템, 고속도로 및 도심 내 차량 운행 분석, 그리고 ADAS(첨단 운전자 지원 시스템) 등입니다.

---

## TAO Toolkit과의 통합

NVIDIA TAO Toolkit은 사전 학습된 모델을 기반으로 전이 학습을 수행하는 프레임워크입니다. LaneNet은 TAO Model Zoo에서 제공되는 모델 중 하나로, 다음과 같은 워크플로우를 따릅니다.

1. **사전학습 모델 다운로드**: `tao model zoo`에서 LaneNet 모델을 다운로드합니다.
2. **데이터셋 준비**: KITTI 형식 또는 커스텀 도로 이미지 데이터셋을 준비합니다.
3. **전이 학습 수행**: 사용자 도메인 데이터로 모델을 튜닝합니다.
4. **추론 및 평가**: IoU 등의 지표로 모델 성능 평가 후 추론을 진행합니다.
5. **최적화 및 배포**: TensorRT 등을 사용하여 모델을 최적화하고, Jetson과 같은 엣지 디바이스에 배포합니다.

---

## 설치 및 준비사항

- **필수 조건**
  - NVIDIA GPU 및 CUDA 지원 드라이버
  - Docker (또는 로컬 Python 환경)
  - NVIDIA TAO Toolkit 설치 ([공식 문서](https://developer.nvidia.com/tao-toolkit) 참고)
  
- **데이터셋 준비**
  - 도로 이미지와 어노테이션 파일 (예: KITTI 형식)
  
---
