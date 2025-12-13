🧭 Autonomous Driving Sensor Systems
Millimeter-Wave Radar + Image Sensor 기반 ADAS / 자율주행 기술 정리

본 문서는 자율주행 자동차 개발에 있어 핵심 센서 기술(특히 mmWave 레이더 + 카메라 조합)을 중심으로
기술 특징, 장단점, 최신 동향, OEM 적용 사례, 주요 벤더, 실제 제품화 업체 등을 총정리한 자료입니다.

📌 Table of Contents

1. 주요 센서 기술 리스트업

1.1 mmWave Radar (24/77/79 GHz)

1.2 이미지 센서 기반 카메라

1.3 LiDAR

1.4 초음파 센서

1.5 IMU / 로컬라이제이션

1.6 GNSS

1.7 V2X 통신

1.8 HD Map 및 센서 퓨전

2. 자동차 회사별 센서 구성 및 적용 예

3. 기술 공급 업체(센서·칩·플랫폼 벤더)

4. 제품 형태로 제공되는 ADAS/자율주행 패키지

5. mmWave 레이더 + 카메라 조합이 가장 합리적인 이유

6. 시스템 설계 시 권장 아키텍처

1. 주요 센서 기술 리스트업
1.1 mmWave Radar (24/77/79 GHz)

원리: RF 반사파로 거리·속도·각도 측정
장점:

악천후(비·눈·안개)에 매우 강함

가격 대비 성능 우수

야간/역광 영향 거의 없음

4D 이미징 레이더로 진화 중 (높이 정보 포함)

단점:

객체 분류(Classification) 능력은 카메라·LiDAR보다 부족

해상도가 낮아 중·근거리 정밀 인식은 카메라 필요

성숙도: ★★★★★ (ADAS 필수 센서)

1.2 이미지 센서 기반 카메라 (Mono / Stereo / Surround)

역할: 차선, 보행자, 표지판, 차량 인식 → 자율주행 인지의 핵심
장점:

텍스처·색상·형태 정보 확보

Lane/Sign/Traffic Light 인식에 필수

고해상도 CMOS 센서(HDR, LED Flicker 억제 등) 발전 중

단점:

악천후/야간 품질 저하

렌즈 오염·눈/비 영향

성숙도: ★★★★★

1.3 LiDAR

장점:

정확하고 고해상도 3D 포인트 클라우드

정밀 지도 기반 로컬라이제이션에 강함

단점:

가격 높음 (단가 수백~수천 USD → 하락 중)

오염/악천후에 취약

성숙도: ★★★★☆ (고급차/플래그십에서 채택 중)

1.4 초음파 센서

용도: 주차, 근거리 장애물 탐지
특징: 저가, 근거리 특화, 이미 시장 성숙

1.5 IMU / 로컬라이제이션

가속도/자이로 + 차량 속도/조향 기반 자세 계산

GNSS 신호 없을 때 Dead-reckoning 역할

고성능 MEMS + Fusion 알고리즘이 핵심

1.6 GNSS

기본 위치 제공

RTK/PPP 기반 → cm 단위까지 가능

도심/터널에서는 신호 불안정

1.7 V2X 통신 (C-V2X / DSRC)

차량 간/차량-인프라 간 통신

“비가시 영역” 위험 탐지 가능

인프라 보급률이 낮아 단독 활용 어려움

1.8 HD Map 및 센서 퓨전 SW

정밀 지도 기반 차선 단위 위치 추정

센서 + 지도 + DNN 기반 Deep Fusion이 최신 방향

ISO 26262 기반 기능 안전 요구

2. 자동차 회사별 센서 구성 및 적용 예
🚗 카메라 + Radar 조합 (대부분의 양산차 기본 구조)
제조사	대표 시스템	센서 구성	대표 차종
Toyota	TSS	단안 카메라 + 전방 레이더	Corolla, Camry, RAV4, Prius
Honda	Honda Sensing	카메라 + 레이더	Civic, Accord, CR-V
Hyundai·Kia	SmartSense	카메라 + 전방/코너 레이더	Sonata, EV6, Ioniq 5/6
Nissan	ProPILOT	카메라 + 레이더	Leaf, Rogue
GM	Super Cruise	카메라 + 레이더 + 맵	Cadillac CT5, Escalade
Ford	BlueCruise	카메라 + 레이더	F-150, Mach-E
VW Group	IQ.Drive	카메라 + 레이더	Golf, Passat, ID.4
BYD	L2+/L2++ ADAS	카메라 + 레이더	Seagull 등 다수
Tesla	Autopilot/FSD	최근 Camera-only 전략	Model 3/Y/S/X
🚘 카메라 + Radar + LiDAR 조합 (고급차/L3 차량 중심)
제조사	센서 구조	적용 모델
Mercedes-Benz	LiDAR + 카메라 + 레이더	S-Class, EQS
Volvo / Polestar	Luminar LiDAR + 카메라 + 레이더	EX90, Polestar 3
중국 OEM(NIO, XPeng 등)	고해상도 LiDAR 다중 배치	L2+/L3 EV 다수
Waymo / Cruise	다중 LiDAR + 카메라 + 레이더	Robotaxi 차량
3. 기술 공급 업체(센서·칩·플랫폼 벤더)
🛰️ mmWave Radar 칩/모듈

Texas Instruments (TI) – AWR series SoC

NXP – 77 GHz 레이더 SoC

Infineon, Analog Devices, Renesas, STMicroelectronics

Tier-1 모듈 업체:
Bosch, Continental, ZF, Aptiv, Valeo, Hella(Forvia), Denso, Hyundai Mobis

📷 이미지 센서(CIS) 및 카메라

onsemi – ADAS 카메라 CIS 1위

OmniVision – 자동차 CIS, 서라운드 뷰

Sony – 고감도/고성능 CIS

Samsung, STMicroelectronics

카메라 모듈/시스템 업체:
Mobileye, Bosch, Continental, Aptiv, Hyundai Mobis

🔦 LiDAR 벤더

Luminar, Valeo, Innoviz, Hesai, Velodyne/Ouster, Aeva, Aeye

🧠 ADAS/자율주행 SoC 플랫폼

NVIDIA – DRIVE Orin / Thor

Qualcomm – Snapdragon Ride / Ride Flex

Mobileye – EyeQ 시리즈

Huawei MDC, Tesla FSD Computer

4. 제품 형태로 제공되는 ADAS/자율주행 패키지
Tier-1의 통합 솔루션

Bosch – 레이더·카메라·ECU·소프트웨어 풀 패키지

Continental – ADAS 센서 + 컨트롤 유닛

Denso, Aptiv, Valeo, ZF, Hyundai Mobis

Full-stack 자율주행 플랫폼

Mobileye SuperVision / Chauffeur

NVIDIA DRIVE AV / Hyperion

Qualcomm Ride Platform

Baidu / Huawei / Momenta(중국)

Waymo, Cruise (Robotaxi용)

5. mmWave 레이더 + 카메라 조합이 가장 합리적인 이유
✔ 비용 대비 성능이 가장 뛰어난 구조

LiDAR 대비 가격 매우 저렴 → 대중차 양산에 최적

✔ 상호 보완적 센서 조합

카메라 → 객체 인식/차선/신호

레이더 → 거리/속도/악천후 보조

두 센서의 융합으로 안전성 향상

✔ 이미 글로벌 OEM 표준

L1~L2+ 차량 대부분이 이 조합을 채택
(LiDAR는 고급/L3 트림에만 적용)

6. 시스템 설계 시 권장 아키텍처
[Sensors]
 ├─ Front Camera (Mono or Stereo)
 ├─ Long-Range Radar (77 GHz)
 ├─ Corner Radar (SRR)
 ├─ Surround Cameras (optional)
 ├─ Ultrasonic Sensors (parking)
 └─ GNSS + IMU

[Perception]
 ├─ Camera DNN (객체/차선/신호 인식)
 ├─ Radar Tracking (거리·속도 추정)
 ├─ Sensor Fusion (Kalman / Deep Fusion)

[Planning & Control]
 ├─ Behavior Planner
 ├─ Trajectory Planner
 └─ Vehicle Control (Steering, Brake, Throttle)

[Compute Platform]
 ├─ NVIDIA / Qualcomm / Mobileye / Custom SoC
 └─ ISO 26262 Functional Safety 적용


📘 문의 / 확장 가능 내용

필요하시다면 아래 자료도 추가 제작해드립니다.

L2+ ADAS 기준 완전한 시스템 블록도

센서 배치 설계 가이드 (FOV, Blind Spot 분석)

mmWave Radar 기반 Target Tracking 알고리즘 설명

카메라 인지 DNN 구조(ResNet/YOLO 기반)

센서 퓨전(EKF/UKF/Deep Fusion) 코드 템플릿

개발 로드맵(1~3년 계획)
