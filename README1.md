# ✅ 1. L2+ ADAS 기준 완전한 시스템 블록도 (Full System Architecture)

* L2+ ADAS는 센서 인지 → 객체 추적 → 센서 퓨전 → 행동 계획 → 궤적 생성 → 차량 제어까지의 전체 체계를 필요로 한다.
* 아키텍처는 크게 4개 계층으로 구성된다:

## 📌 Layer 1: Sensor Layer (Raw Data Acquisition)

### 센서 종류 및 역할:

|센서	|주요 역할	|특징|
|---------|---------|---------|
|Front Camera	|객체, 차선, 신호등 인식|	DNN 기반 인지 핵심|
|Long-Range Radar (LRR)	|거리·속도 추정, ACC/AEB	|악천후 강함, 거리 정확|
|Corner Radar (SRR/MRR)	|BSD, LCA, 교차로 감시	|FOV 넓음|
|Surround Camera	|360° 인지(후방, 측방)	|차선 변경/후방 안전|
|Ultrasonic	|근거리 주차 및 저속 장애물	|0–5m|
|GNSS + IMU	|차량 절대/상대 위치	|Dead-reckoning|
|LiDAR (옵션)	|고정밀 3D 인지	|L3 이상에서 중요|

## 📌 Layer 2: Perception Layer (Sensor Processing)

* Perception은 카메라와 레이더가 독립적으로 인지한 결과물을 객체 기반 정보로 변환한다.

**Camera Perception**
- Object Detection (YOLO/CenterNet/DETR)
- Lane Detection (SCNN/PolyLaneNet)
- Traffic Light/Sign Recognition
- Free-Space Segmentation
- Radar Signal Processing
- Range FFT, Doppler FFT, Angle FFT
- CFAR Detection
- Clustering
- Radar Tracklet 생성 (object-level 추적 이전의 점 집합)

## 📌 Layer 3: Sensor Fusion & Planning Layer
Sensor Fusion (Multiple Modalities)

Camera 객체 + Radar 거리/속도

Kalman Filter / UKF 기반 Object Fusion

Deep Fusion 기반 Feature-level Fusion

Behavior Planning

ACC Distance Keeping

Cut-in / Cut-out 판단

Overtake / Lane-change Decision

Collision Avoidance Behavior

Trajectory Planning

Polynomial / Spline trajectory

Speed Profile Generation

Lateral & Longitudinal Control Path 생성

## 📌 Layer 4: Vehicle Control Layer

Steering Control (LQR, Stanley, MPC 등)

Brake Control

Throttle Control

Actuator Feedback Loop

## 📌 L2+ Full Architecture Diagram (텍스트 버전)
[Sensors]
  Camera | LRR | SRR | Ultrasonic | GNSS | IMU | (LiDAR optional)
          ↓         ↓
[Perception]
  Camera DNN  +  Radar Tracking
          \        /
           \      /
        [Sensor Fusion (EKF/UKF/Deep)]
                    ↓
            [Behavior Planning]
                    ↓
            [Trajectory Planning]
                    ↓
            [Vehicle Control ECU]
                    ↓
           Steering / Brake / Accel

# ✅ 2. 센서 배치 설계 가이드 (FOV, Blind Spot 분석)

L2+ 수준에서는 센서 배치가 인지 성능을 크게 좌우한다.

## 📌 2.1 Front Camera 배치 규칙

장착 높이: 1.2–1.5 m

Pitch 각도: -1° ~ +2° (지면을 약간 내려다보도록)

FOV: 60°–120°

목표:

보행자/차량 인식

차선 인식

신호등/표지판 인식

## 📌 2.2 Long-Range Radar (LRR) 배치

높이: 0.4–0.7 m

FOV: 10°–20° (좁고 멀리 보는 구조)

Range: 최대 250m

목표:

ACC (Adaptive Cruise Control)

AEB (Automatic Emergency Braking)

Cut-in 차량 조기 감지

LRR은 오염(눈/진흙)에 대한 영향이 적어 범퍼 내부에 배치한다.

## 📌 2.3 Corner Radar (SRR/MRR) 배치

FOV: 90°–150°

Range: 20–80m

역할:

BSD (Blind Spot Detection)

LCA (Lane Change Assist)

CTA (Cross Traffic Alert)

차량 4모서리에 배치하여 전·후측면의 사각 영역을 제거한다.

## 📌 Blind Spot Diagram
             FRONT
               ^
               |
       [SRR]  Car  [SRR]
           \       /
            \     /
         Blind Spots

# ✅ 3. mmWave Radar 기반 Target Tracking 알고리즘 설명

Radar Tracking은 크게 **신호 처리(Signal domain)**와 **추적(Track domain)**으로 나뉜다.

## 📌 3.1 신호 처리 단계 (Signal Processing)
Step 1: Range FFT

거리(Range) 축으로 FFT 수행하여 반사체 위치 추정.

Step 2: Doppler FFT

속도(상대 속도) 추출.

Step 3: Angle Estimation (AoA)

MIMO 레이아웃 기반 멀티채널 위상차로 각도(angle) 추정.

Step 4: Range-Doppler Map & 3D Cube 생성

## 📌 3.2 CFAR (Constant False Alarm Rate) Detection

노이즈 기반 threshold를 동적으로 계산하여 실제 target peak만 검출.

CA-CFAR

OS-CFAR

GO-CFAR

## 📌 3.3 Clustering

검출된 점들을 실제 object로 묶는 과정.

DBSCAN

K-means

## 📌 3.4 Tracking (추적 필터)
Kalman Filter

차량의 움직임이 선형에 가까울 때 유리

도심 자동주행에서 널리 사용

UKF / IMM (Interacting Multiple Model)

곡선/고속 상황에서 더 안정적

모델 전환(정속/가속/선회) 가능

## 📌 Tracking Pipeline 요약
ADC → FFT → CFAR → Clustering → Track Association → Kalman/UKF Tracking

# ✅ 4. 카메라 인지 DNN 구조 (ResNet / YOLO 기반)

# 📌 4.1 YOLO 기반 구조 설명

YOLO는 End-to-End Real-time Object Detection 구조이다.

Input → Backbone(CSPDarkNet/ResNet) → Neck(PANet/FPN) → Head

Backbone 역할:

Feature extraction

ResNet, CSPDarkNet53 등이 대표

Neck 역할:

Multi-scale Feature Fusion

PANet / FPN 구조

Head 역할:

Bounding Box 회귀

Class 예측

Confidence score 계산

## 📌 4.2 YOLO Example Architecture Diagram
[Input 1280x720]
        ↓
[CSPDarkNet / ResNet Backbone]
        ↓
[FPN/PANet Feature Fusion]
        ↓
[Detection Head]
  ├─ Car
  ├─ Pedestrian
  ├─ Cyclist

## 📌 4.3 Multi-task Camera Perception (ADAS 특화)

카메라 1대에서 아래 기능을 동시에 수행할 수 있다:

Vehicle detection

Pedestrian detection

Lane detection

Traffic light recognition

Traffic sign classification

Drivable area segmentation

# ✅ 5. 센서 퓨전(EKF / UKF / Deep Fusion) 코드 템플릿

## 📌 5.1 EKF (Extended Kalman Filter) 기본 구조
def ekf_update(x, P, z, H, R):
    y = z - H @ x
    S = H @ P @ H.T + R
    K = P @ H.T @ np.linalg.inv(S)
    x = x + K @ y
    P = (np.eye(len(x)) - K @ H) @ P
    return x, P

## 📌 5.2 UKF (Unscented Kalman Filter) 핵심 요약

UKF는 비선형 모델에서 더 높은 정확도를 제공.

절차:

Sigma point 생성

비선형 함수에 통과

예측 결과 평균/공분산 계산

Update 단계 수행

## 📌 5.3 Deep Fusion 구조 (Feature-level Fusion)

카메라 + 레이더 feature 레벨에서 딥러닝 기반으로 융합하는 방식.

Camera Feature → CNN → Fc_C
Radar Feature → 1D-CNN → Fc_R

Fusion:
  F = MLP([Fc_C, Fc_R])


Deep Fusion은 L2+ 객체 인식 성능을 획기적으로 올리는 최신 구조이다.

# ✅ 6. ADAS 개발 로드맵 (1~3년 계획)

ADAS 개발은 센서 인지 → Fusion → Planning → 실도로 검증 순으로 진행해야 한다.

📅 Year 1 — Perception & Fusion Foundation

Radar + Camera Perception 기본 구축

Object detection + Radar CFAR tracking

EKF 기반 Object-level Sensor Fusion

ACC / AEB / LKA 등 핵심 ADAS 기능 구현

HIL Simulation 구축

📅 Year 2 — System Enhancement

4D Imaging Radar 적용

Deep Fusion 적용

Overtake / Cut-in / Cut-out planning 강화

스플라인/최적화 기반 Trajectory Planner 도입

기능 안전(ISO 26262) 대응 시작

📅 Year 3 — L2+/L3 Ready Platform

Redundancy architecture 설계

LiDAR 옵션 적용 가능 구조 확보

OTA + Continual Learning 체계 구축

도심 환경에서의 고난도 위험 상황 대응

Mass-production (양산) Validation 프로세스 정립

🎯 결론

이제 위 6개 항목은 README.md 때문에 축소된 버전이 아니라,
내용에 집중하는 완전한 엔지니어링 문서 레벨로 정리되었어.
