# Nvidia-tao-Lanenet
## "Lane net" 이란?
--> Nvidia TAO Toolkit에서 제공하는 차선 인식 모델, 자율주행 차량 등에 매우 유용하다.
# LaneNet: NVIDIA TAO Lane Detection Model

LaneNet은 NVIDIA TAO Toolkit 기반의 **차선 검출 모델(Lane Detection)** 이다. 이 모델은 도로 이미지에서 차선을 픽셀 단위로 분리하여, 자율주행, ADAS 등 다양한 운전자 지원 시스템에서 활용할 수 있도록 설계되었다.

---

## 목차

- [특징](#%ED%8A%B9%EC%A7%95)
- [개요](#%EA%B0%9C%EC%9A%94)
- [TAO Toolkit과의 통합](#tao-toolkit%EC%99%80%EC%9D%98-%ED%86%B5%ED%95%A9)

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
## Tao LaneNet 예시 코드 구조 

### 1. Spec 파일 예시 (train_spec.txt)
```
dataset_config {
  data_sources: {
    label_directory_path: "/workspace/tao-experiments/lanenet/train/labels"
    image_directory_path: "/workspace/tao-experiments/lanenet/train/images"
  }
  image_extension: "jpg"
  target_class_mapping {
    key: "lane"
    value: "lane"
  }
  validation_data_sources: {
    label_directory_path: "/workspace/tao-experiments/lanenet/val/labels"
    image_directory_path: "/workspace/tao-experiments/lanenet/val/images"
  }
}

model_config {
  input_image_width: 1280
  input_image_height: 720
  input_image_channels: 3
  anchor_stride: 16
}

train_config {
  batch_size: 4
  epochs: 80
  learning_rate {
    soft_start_annealing_schedule {
      min_learning_rate: 1e-5
      max_learning_rate: 1e-3
      soft_start: 0.1
      annealing: 0.7
    }
  }
  regularizer {
    type: L1
    weight: 5e-4
  }
}

augmentation_config {
  output_width: 1280
  output_height: 720
  horizontal_flip: 0.5
  crop_and_resize_prob: 0.5
  zoom_min: 1.0
  zoom_max: 2.0
}
```

### 2. 학습 명령어 예시 
```
tao lanenet train \
  -e /path/to/train_spec.txt \
  -r /results/lanenet/ \
  -k <encryption_key>
```

### 3. 추론 명령어 예시
```
tao lanenet inference \
  -e /path/to/infer_spec.txt \
  -i /path/to/test/images \
  -o /path/to/output \
  -m /path/to/trained_model.tlt \
  -k <encryption_key>
```

### 4. 모델 export 예시
```
tao lanenet export \
  -m /path/to/trained_model.tlt \
  -o /path/to/model.onnx \
  -k <encryption_key>
```



