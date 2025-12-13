# 🧭 Autonomous Driving Sensor Systems
Millimeter-Wave Radar + Image Sensor 기반 ADAS / 자율주행 기술 정리

본 문서는 자율주행 자동차 개발에 있어 핵심 센서 기술(특히 mmWave 레이더 + 카메라 조합)을 중심으로  
기술 특징, 장단점, 최신 동향, OEM 적용 사례, 주요 벤더, 실제 제품화 업체 등을 총정리한 자료입니다.

---

# 📌 Table of Contents

**1. 주요 센서 기술 리스트업**
  * 1.1 mmWave Radar (24/77/79 GHz)
  * 1.2 이미지 센서 기반 카메라
  * 1.3 LiDAR
  * 1.4 초음파 센서
  * 1.5 IMU / 로컬라이제이션
  * 1.6 GNSS
  * 1.7 V2X 통신
  * 1.8 HD Map 및 센서 퓨전
* 2. 자동차 회사별 센서 구성 및 적용 예  
* 3. 기술 공급 업체(센서·칩·플랫폼 벤더)  
* 4. 제품 형태로 제공되는 ADAS/자율주행 패키지  
* 5. mmWave 레이더 + 카메라 조합이 가장 합리적인 이유  
* 6. 시스템 설계 시 권장 아키텍처  
* 7. L2+ ADAS 기준 시스템 블록도  
* 8. 센서 배치 설계 가이드 (FOV / Blind Spot 분석)  
* 9. mmWave Radar Target Tracking 알고리즘  
* 10. 카메라 인지 DNN 구조 (ResNet/YOLO 기반)  
* 11. 센서 퓨전(EKF/UKF/Deep Fusion) 코드 템플릿  
* 12. ADAS / L2+ 개발 로드맵 (1~3년 계획)

---

# 1. 주요 센서 기술 리스트업

## 1.1 mmWave Radar (24/77/79 GHz)
* 원리: RF 반사파로 거리·속도·각도 측정
* 장점:
   * 악천후(비·눈·안개)에 매우 강함
   * 가격 대비 성능 우수
   * 야간/역광 영향 거의 없음
   * 4D 이미징 레이더로 진화 중 (높이 정보 포함)
* 단점:
   * 객체 분류(Classification) 능력은 카메라·LiDAR보다 부족
   * 해상도가 낮아 중·근거리 정밀 인식은 카메라 필요
* 성숙도: ★★★★★ (ADAS 필수 센서)

## 1.2 이미지 센서 기반 카메라 (Mono/Stereo/Surround)
객체·차선·신호 인식, 고해상도 CMOS 발전  
조도 민감·렌즈 오염 취약  
성숙도: ★★★★★

## 1.3 LiDAR
고해상도 3D 인지, 고가/악천후 취약  
성숙도: ★★★★☆

## 1.4 초음파 센서
주차/근거리 탐지 용도, 매우 성숙

## 1.5 IMU / 로컬라이제이션
Dead-reckoning, 고성능 MEMS 필요

## 1.6 GNSS
RTK/PPP 기반 cm급 가능, 도심 취약

## 1.7 V2X 통신
비가시 영역 보조, 인프라 의존

## 1.8 HD Map 및 센서 퓨전
정밀 위치 추정, 최신 자율주행 핵심

---

# 2. 자동차 회사별 센서 구성 및 적용 예

## 카메라 + Radar 기반 (대부분 양산차)
Toyota TSS / Honda Sensing / Hyundai-Kia SmartSense / Nissan ProPILOT / GM Super Cruise / Ford BlueCruise / VW IQ.Drive / BYD L2+ / Tesla Vision

## 카메라 + Radar + LiDAR (고급차/L3 준비)
Mercedes Drive Pilot / Volvo EX90 / Polestar 3 / NIO·XPeng·Li Auto / Waymo·Cruise Robotaxi

---

# 3. 기술 공급 업체(센서·칩·플랫폼 벤더)

## Radar 칩
TI, NXP, Infineon, ADI, Renesas, ST

## Camera CIS
onsemi, OmniVision, Sony, Samsung

## LiDAR
Luminar, Hesai, Innoviz, Ouster

## ADAS/AV SoC
NVIDIA Orin/Thor, Qualcomm Ride, Mobileye EyeQ, Huawei MDC

---

# 4. 제품 형태로 제공되는 ADAS/자율주행 패키지
Bosch / Continental / Valeo / Hyundai Mobis / Aptiv / Denso  
Mobileye SuperVision / NVIDIA Hyperion / Qualcomm Ride

---

# 5. mmWave 레이더 + 카메라 조합이 가장 합리적인 이유
- 비용 대비 최고 효율  
- 악천후 대응  
- 객체+거리 보완 구조  
- L1~L2+ 글로벌 표준 구성  

---

# 6. 시스템 설계 시 권장 아키텍처

```
[Sensors]
 ├─ Front Camera (Mono/Stereo)
 ├─ Long-Range Radar (77 GHz)
 ├─ Corner Radar (4ea)
 ├─ Surround Cameras
 ├─ Ultrasonic Sensors
 └─ GNSS + IMU

[Perception]
 ├─ Camera DNN
 ├─ Radar Tracking
 └─ Sensor Fusion

[Planning]
 ├─ Behavior Planner
 └─ Trajectory Planner

[Control]
 └─ Vehicle Actuation (Steer/Brake/Accel)
```

---

# 7. L2+ ADAS 기준 시스템 블록도

```
+---------------------------------------------------------------+
|                           Sensor Layer                        |
+---------------------------------------------------------------+
|  Camera | LRR | CRR | Ultrasonic | GNSS | IMU | Optional LiDAR |
+---------------------------------------------------------------+

+----------------+      +-----------------------------+
| Camera DNN     |      | Radar Signal Processing     |
| Object/Lane    |      | FFT → CFAR → Clustering     |
+-------+--------+      +--------------+--------------+
        |                              |
        +------------------------------+
                       |
            +----------------------------+
            |   Sensor Fusion (EKF/UKF)  |
            +-------------+--------------+
                          |
            +----------------------------+
            |    Behavior Planning        |
            +-------------+--------------+
                          |
            +----------------------------+
            |     Trajectory Planning     |
            +-------------+--------------+
                          |
            +----------------------------+
            |   Vehicle Control (ECU)     |
            +----------------------------+
```

---

# 8. 센서 배치 설계 가이드 (FOV / Blind Spot 분석)

## Front Camera
- 높이: 1.2–1.5 m  
- FOV: 60°–120°

## Long-Range Radar
- 낮은 FOV, 150–250m 탐지  
- 범퍼 중앙 배치

## Corner Radar
- 90° 이상 넓은 FOV  
- BSD/LCA 용도

### Blind Spot Diagram
```
        Front
         ^
   [CR] Car [CR]
      \     /
     Blind Spots
```

---

# 9. mmWave Radar Target Tracking Algorithm

## Processing Pipeline
1. Range FFT  
2. Doppler FFT  
3. CFAR Detection  
4. Clustering (DBSCAN)  
5. Tracking (Kalman Filter)  

```python
def radar_pipeline(raw_adc):
    rd = range_doppler_fft(raw_adc)
    det = cfar_detection(rd)
    clusters = dbscan(det)
    tracks = kalman_tracking(clusters)
    return tracks
```

---

# 10. Camera DNN Architecture (YOLO/ResNet 기반)

```
Input → Backbone(CSPDarkNet/ResNet) → Neck(PANet/FPN) → Head(detector/lane)
```

```yaml
model:
  backbone: CSPDarkNet53
  head:
    classes: [car, pedestrian, cyclist]
```

---

# 11. Sensor Fusion Templates (EKF / UKF / Deep Fusion)

## EKF Python Template
```python
def ekf_update(x, P, z, H, R):
    y = z - H @ x
    S = H @ P @ H.T + R
    K = P @ H.T @ np.linalg.inv(S)
    x = x + K @ y
    P = (np.eye(len(x)) - K @ H) @ P
    return x, P
```

## Deep Fusion
Camera Feature + Radar Feature → MLP Fusion

---

# 12. ADAS / L2+ 개발 로드맵 (1~3년)

## Year 1
- Camera + Radar Perception  
- EKF Fusion  
- ACC, AEB, LKA 구현  

## Year 2
- 4D Radar 적용  
- Deep Fusion 기반 Perception 고도화  

## Year 3
- L3-ready Architecture  
- OTA 기반 학습  
- Redundant Sensor 구조 적용  

---

# END OF DOCUMENT
