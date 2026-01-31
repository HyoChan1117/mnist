# MNIST 손글씨 숫자 인식

MNIST 데이터셋을 활용하여 CNN 기반 손글씨 숫자 인식 모델을 학습하고,  
Web 환경에서 실시간으로 예측 결과를 확인할 수 있는 AI 서비스를 구현한 프로젝트입니다.

---

## 프로젝트 개요

### MNIST란?

손글씨 숫자(0~9)로 구성된 대표적인 이미지 데이터셋입니다.

- 28×28 크기의 흑백 이미지
- 딥러닝 학습 및 실습에 가장 널리 사용
- 빠른 학습 및 테스트 가능

---

### 프로젝트 목표

Web 기반 손글씨 숫자 인식 AI 서비스 구축

- 딥러닝 모델 학습 및 성능 검증  
- ONNX 변환을 통한 모델 배포 과정 이해  
- 실시간 예측이 가능한 Web 시스템 구현  

학습부터 서비스 제공까지 AI 시스템 전체 흐름 이해

---

## 기능

- CNN 모델 학습 (PyTorch)
- ONNX 변환 및 ONNX Runtime 추론
- Flask REST API (`/predict`)
- 웹 캔버스에서 직접 손글씨 입력 및 인식

## 프로젝트 구조

```
mnist/
├── 01_train.py          # 모델 학습
├── 02_export_onnx.py    # ONNX 변환
├── 03_server.py         # Flask 서버
├── static/
│   └── mnist.html       # 손글씨 캔버스 UI
├── models/
│   ├── mnist.pth        # PyTorch 모델
│   └── mnist.onnx       # ONNX 모델
├── requirements.txt
└── render.yaml          # Render 배포 설정
```

## 설치

```bash
# 가상환경 생성
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate

# 의존성 설치
pip install -r requirements.txt
```

## 사용법

### 1. 모델 학습

```bash
python 01_train.py
```

학습된 모델이 `models/mnist.pth`에 저장됩니다.

### 2. ONNX 변환

```bash
python 02_export_onnx.py
```

ONNX 모델이 `models/mnist.onnx`에 저장됩니다.

### 3. 서버 실행

```bash
python 03_server.py
```

- 메인 페이지: http://localhost:5000
- 손글씨 캔버스: http://localhost:5000/contents/mnist.html

### API 사용

```bash
curl -X POST -F "file=@digit.png" http://localhost:5000/predict
```

응답 예시:
```json
{
  "prediction": 7,
  "probabilities": [0.01, 0.02, ...]
}
```

## 배포 (Render)

1. GitHub에 푸시
2. [Render](https://render.com)에서 Web Service 생성
3. 레포지토리 연결 후 배포

`render.yaml` 설정이 자동으로 적용됩니다.

## 모델 구조

```
Conv2d(1, 32) → ReLU → MaxPool2d
Conv2d(32, 64) → ReLU → MaxPool2d
Flatten → Linear(3136, 128) → ReLU → Linear(128, 10)
```

## 기술 스택

- **학습**: PyTorch, torchvision
- **추론**: ONNX Runtime
- **서버**: Flask, Gunicorn
- **배포**: Render
