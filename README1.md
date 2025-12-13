# 1. 자율주행용 센서/시스템 기술 정리

(특히 mmWave 레이더 + 이미지 센서 관점)

---

## 1-1. 주요 센서/시스템 기술
🚗 자율주행 센서 기술 비교 및 동향

| 기술 | 원리/역할 | 장점 | 단점 | 성숙도 & 최신 동향 |
| :--- | :--- | :--- | :--- | :--- |
| **mmWave 레이더 (24/77/79 GHz)** | RF 신호를 쏘고 반사파를 측정해 거리·속도·각도 추출. 77 GHz 3D/4D 이미징 레이더가 주류. | • 거리·속도 측정 **정확**, **악천후** (비·안개·눈)에 **강함** | • 해상도가 카메라/LiDAR보다 **낮음** (객체 분류) | **아주 성숙** (ACC, AEB 필수 센서). 77GHz 통일, **4D 이미징 레이더** (고해상도 + 높이 정보)로 진화 중. |
| | | • 밤/눈부심 무관 | • 금속/큰 물체에 유리, 보행자/자전거 분류 어려움 | |
| | | • 가격이 LiDAR보다 **저렴**, 모듈 작음 | • **간섭 문제** (주파수 혼잡) | |
| **이미지 센서 기반 카메라** (Mono/ Stereo/ Surround) | CMOS 센서로 2D/3D 영상 취득, DNN으로 차선·보행자·차량·표지판 인식 | • 공간 해상도 **높고** 시각 정보 (텍스처, 색상) **풍부** | • **빛 의존**: 역광, 야간, 안개, 눈/비에서 성능 저하 | **매우 성숙** + 계속 진화 중. HDR, LED 플리커 억제, 야간용 NIR/IR 센서 기술 발전. |
| | | • 차선/표지판/신호등 등 **규정 기반 인지 필수** | • 눈/얼음/오염에 취약 | |
| | | • HW/렌즈 가격 하락 | • **연산량** (SoC, GPU) 큼 | |
| **LiDAR** | 레이저 펄스의 TOF/위상으로 **3D 포인트클라우드** 생성 | • **거리 정확도**와 **각도 해상도 최고 수준** | • **고가** (하락 추세) | 기술적으로는 성숙에 가까우나 원가/패키징이 과제. Mercedes-Benz, Volvo 등 고급차/L3 이상에서 채택 확대. |
| | | • **복잡한 3D 환경 인지**에 유리 | • 악천후/더스트에서 성능 저하 | |
| | | • 카메라/레이더 보완 | • 광학 윈도우 오염에 민감 | |
| **초음파 센서** | 수십 kHz 초음파 반사로 **근거리** (수십 cm) 장애물 탐지 | • 아주 **저렴**, 주차·저속 근접검지에 **효과적** | • 거리/각도 해상도 낮음 | **완전 성숙**, 주차 보조용 표준. 고급 차에서는 레이더/카메라로 일부 대체 중. |
| | | • 금속/비금속 모두 측정 가능 | • 고속/중·장거리 용도 **부적합** | |
| **IMU + 차량 위치 센서** (휠속, 조향각) | 가속도/자이로 + 휠/조향 센서로 자세/궤적 추정 | • 고속/단기 위치추정에 **안정적** | • **드리프트 누적**, 단독 사용 불가 | **성숙**. 주행제어/ESP 필수. 고성능 MEMS + 소프트웨어로 정확도 개선. |
| | | • GNSS/맵과 융합 시 정밀 로컬라이제이션에 **핵심** | | |
| **GNSS** (GPS/GLONASS/BeiDou 등) | 위성 신호로 **절대 위치** 파악 | • 전 지구 위치 파악, 고속도로 주행 등에서 **필수** | • 도심 캐니언, 터널 등에서 **신호 불안정** | RTK/PPP, 다중 GNSS, 고정밀 맵과의 융합으로 **cm급까지 향상** 중. |
| | | | • 단독 위치 정확도 제한 (수 m) | |
| **V2X** (V2V/V2I, DSRC/ C-V2X) | 차량·인프라 간 통신으로 **비가시 영역 정보** 공유 | • **“보이지 않는 위협”** (코너 뒤 차량 등) 탐지 가능 | • 인프라/보급률 의존, 보안/표준 문제 | 일부 국가/도시에서 인프라 시험/초기 상용 단계. 장기적인 센서 보조 수단. |
| | | • 교차로 안전, 군집주행 등에 **강점** | • 바로 상용화된 완전 자율주행 필수요소는 아님 | |
| **고정밀 HD 맵 + 로컬라이제이션** | 선행 계측된 고해상도 지도를 이용한 **정밀 위치결정** | • **차선 단위 위치**, 구조물/신호 위치 정보 제공 | • **갱신 비용**, 도로 공사/변경에 취약 | L3 이상/로보택시에서 중요. 최근에는 **“맵 의존 감소”** (온라인 갱신 + 센서 기반 동적 지도) 트렌드. |
| | | • 센서 인지 부담 감소 | • 글로벌 서비스 확장 어려움 | |
| **센서 퓨전/Perception SW** | 위 센서들의 정보를 통합해 객체/차선/Free-space 인지 | • **단일 센서의 한계를 보완** | • 아키텍처 복잡, 개발/검증 비용 큼 | **최근 가장 뜨거운 영역**. Kalman/Particle Filter + DNN 기반 **딥퓨전**까지 다양. |
| | | • 악천후/센서 장애에 대한 **안전성** (Functional Safety) 향상 | • **고성능 컴퓨팅** 필요 | |

---

## 2. 실제 양산차 적용 예 (회사/기종, 센서 구성)
완전한 리스트는 너무 방대하니, 대표 OEM + 대표 차종 + 센서 구성 위주로만 적겠습니다. (연식·트림에 따라 다를 수 있음)

---

### 2-1. 카메라 + mmWave 레이더 기반 (LiDAR 비포함)

| 기술 | 원리/역할 | 장점 | 단점 | 성숙도 & 최신 동향 |
| :--- | :--- | :--- | :--- | :--- |
| **mmWave 레이더 (24/77/79 GHz)** | RF 신호를 쏘고 반사파를 측정해 거리·속도·각도 추출. 77 GHz 3D/4D 이미징 레이더가 주류. | • 거리·속도 측정 **정확**, **악천후** (비·안개·눈)에 **강함** | • 해상도가 카메라/LiDAR보다 **낮음** (객체 분류) | **아주 성숙** (ACC, AEB 필수 센서). 77GHz 통일, **4D 이미징 레이더** (고해상도 + 높이 정보)로 진화 중. |
| | | • 밤/눈부심 무관 | • 금속/큰 물체에 유리, 보행자/자전거 분류 어려움 | |
| | | • 가격이 LiDAR보다 **저렴**, 모듈 작음 | • **간섭 문제** (주파수 혼잡) | |
| **이미지 센서 기반 카메라** (Mono/ Stereo/ Surround) | CMOS 센서로 2D/3D 영상 취득, DNN으로 차선·보행자·차량·표지판 인식 | • 공간 해상도 **높고** 시각 정보 (텍스처, 색상) **풍부** | • **빛 의존**: 역광, 야간, 안개, 눈/비에서 성능 저하 | **매우 성숙** + 계속 진화 중. HDR, LED 플리커 억제, 야간용 NIR/IR 센서 기술 발전. |
| | | • 차선/표지판/신호등 등 **규정 기반 인지 필수** | • 눈/얼음/오염에 취약 | |
| | | • HW/렌즈 가격 하락 | • **연산량** (SoC, GPU) 큼 | |
| **LiDAR** | 레이저 펄스의 TOF/위상으로 **3D 포인트클라우드** 생성 | • **거리 정확도**와 **각도 해상도 최고 수준** | • **고가** (하락 추세) | 기술적으로는 성숙에 가까우나 원가/패키징이 과제. Mercedes-Benz, Volvo 등 고급차/L3 이상에서 채택 확대. |
| | | • **복잡한 3D 환경 인지**에 유리 | • 악천후/더스트에서 성능 저하 | |
| | | • 카메라/레이더 보완 | • 광학 윈도우 오염에 민감 | |
| **초음파 센서** | 수십 kHz 초음파 반사로 **근거리** (수십 cm) 장애물 탐지 | • 아주 **저렴**, 주차·저속 근접검지에 **효과적** | • 거리/각도 해상도 낮음 | **완전 성숙**, 주차 보조용 표준. 고급 차에서는 레이더/카메라로 일부 대체 중. |
| | | • 금속/비금속 모두 측정 가능 | • 고속/중·장거리 용도 **부적합** | |
| **IMU + 차량 위치 센서** (휠속, 조향각) | 가속도/자이로 + 휠/조향 센서로 자세/궤적 추정 | • 고속/단기 위치추정에 **안정적** | • **드리프트 누적**, 단독 사용 불가 | **성숙**. 주행제어/ESP 필수. 고성능 MEMS + 소프트웨어로 정확도 개선. |
| | | • GNSS/맵과 융합 시 정밀 로컬라이제이션에 **핵심** | | |
| **GNSS** (GPS/GLONASS/BeiDou 등) | 위성 신호로 **절대 위치** 파악 | • 전 지구 위치 파악, 고속도로 주행 등에서 **필수** | • 도심 캐니언, 터널 등에서 **신호 불안정** | RTK/PPP, 다중 GNSS, 고정밀 맵과의 융합으로 **cm급까지 향상** 중. |
| | | | • 단독 위치 정확도 제한 (수 m) | |
| **V2X** (V2V/V2I, DSRC/ C-V2X) | 차량·인프라 간 통신으로 **비가시 영역 정보** 공유 | • **“보이지 않는 위협”** (코너 뒤 차량 등) 탐지 가능 | • 인프라/보급률 의존, 보안/표준 문제 | 일부 국가/도시에서 인프라 시험/초기 상용 단계. 장기적인 센서 보조 수단. |
| | | • 교차로 안전, 군집주행 등에 **강점** | • 바로 상용화된 완전 자율주행 필수요소는 아님 | |
| **고정밀 HD 맵 + 로컬라이제이션** | 선행 계측된 고해상도 지도를 이용한 **정밀 위치결정** | • **차선 단위 위치**, 구조물/신호 위치 정보 제공 | • **갱신 비용**, 도로 공사/변경에 취약 | L3 이상/로보택시에서 중요. 최근에는 **“맵 의존 감소”** (온라인 갱신 + 센서 기반 동적 지도) 트렌드. |
| | | • 센서 인지 부담 감소 | • 글로벌 서비스 확장 어려움 | |
| **센서 퓨전/Perception SW** | 위 센서들의 정보를 통합해 객체/차선/Free-space 인지 | • **단일 센서의 한계를 보완** | • 아키텍처 복잡, 개발/검증 비용 큼 | **최근 가장 뜨거운 영역**. Kalman/Particle Filter + DNN 기반 **딥퓨전**까지 다양. |
| | | • 악천후/센서 장애에 대한 **안전성** (Functional Safety) 향상 | • **고성능 컴퓨팅** 필요 | |

---

### 2-2. 카메라 + 레이더 + LiDAR 기반 (주로 L3/L4 지향)

| 제조사 / 서비스 | 시스템 명칭 / 레벨 | 센서 구성 | 주요 차종 / 플랫폼 | 비고 및 특징 |
| :--- | :--- | :--- | :--- | :--- |
| **Mercedes-Benz** | Drive Pilot (**L3**) | 다중 **카메라** + 여러 **레이더** + 전방 **LiDAR** + **초음파** + **고정밀 맵**. | S-Class, EQS 일부 트림에 적용, 향후 확대 계획. | **세계 최초 L3 조건부 자율주행** 인증 (특정 조건에서 운전자의 시선 회피 허용). **LiDAR**가 안전성 확보에 핵심 역할. |
| **Volvo / Polestar** | EX90, Polestar 3 플랫폼 | **Luminar LiDAR** + **카메라** + **레이더** + **초음파**. | Volvo EX90, Polestar 3 등. | **LiDAR를 지붕에 통합**하여 차세대 모델에 기본/옵션으로 채택하여 안전성과 L3 가능성을 높임. |
| **중국 OEM** (NIO, XPeng, Li Auto, Huawei 협력 모델 등) | L2+/L3 ADAS | 상위 트림에서 고해상도 **LiDAR** + 8~11 **카메라** + 5 **레이더** 구성. | NIO ET7/ET5, XPeng P7/P5, Li Auto L9 등. | **LiDAR 탑재**를 통해 ADAS의 인지 능력을 빠르게 향상시키며 **기술 선도 경쟁** 중. |
| **Waymo, Cruise 등** | 로보택시 (**L4**) | **다수의 LiDAR** + **카메라** + **레이더** + **HD맵**. | Waymo Driver, Cruise Origin 등 전용 로보택시 차량. | **실질적인 L4 시스템**. **LiDAR를 주력 인지 센서**로 사용하여 높은 수준의 안정성과 3D 인지 능력을 확보. |

---

## 3. 관련 기술 벤더 (센서/칩/플랫폼 공급사)

---

### 3-1. mmWave 레이더 칩/모듈·시스템 업체

* 반도체/칩 수준
  * Texas Instruments (TI) – AWR1xxx/2xxx/3xxx mmWave SoC (77 GHz radar)
  * NXP – 77 GHz 레이더 트랜시버/MCU, 레이더 SoC, 레퍼런스 디자인
  * Infineon, Analog Devices, Renesas, STMicroelectronics – 레이더 프론트엔드/트랜시버, 전력/아날로그 솔루션.
* Tier-1 레이더 모듈 공급사 (실제 완성차에 들어가는 모듈)
  * Bosch, Continental, Denso, Aptiv, ZF, Valeo, Hella(Forvia), Hyundai Mobis 등.

### 3-2. 이미지 센서/카메라 모듈 공급사

* 이미지 센서 칩 (CIS)
  * onsemi – 자동차 이미지 센서 시장 점유율 1위 (약 40%)
  * OmniVision – ADAS/서라운드뷰에서 빠르게 성장, 2위권.
  * Sony – 고성능/고감도 센서(특히 고급차/서라운드 뷰) 공급.
  * Samsung, STMicroelectronics 등도 자동차용 CIS 공급.

* 카메라 모듈/시스템 업체
  * Mobileye (Intel) – EyeQ SoC + 카메라 기반 ADAS/자율주행 스택, 많은 OEM에 공급.
  * Aptiv, Bosch, Continental, Denso, ZF, Valeo, Hyundai Mobis – 전방 카메라/서라운드 카메라 모듈 및 ADAS ECU.

### 3-3. LiDAR 벤더 (자동차용 중심)
* Valeo (Scala LiDAR) – Audi 등 OEM 양산 적용.
* Luminar – Volvo, Mercedes-Benz, SAIC 등과 파트너십 (Halo, Iris 등).
* Innoviz, Velodyne/Ouster, Hesai, LeddarTech, Aeva, Aeye 등.

### 3-4. 자율주행/ADAS SoC & 플랫폼 벤더
* NVIDIA – DRIVE Orin / Thor : 고성능 GPU 기반 AV 컴퓨팅.
* Qualcomm – Snapdragon Ride / Ride Flex : ADAS/인포테인먼트 통합 SoC.
* Mobileye – EyeQ 시리즈 : 카메라 위주의 ADAS/AV SoC 및 소프트웨어 스택.
* Tesla, Huawei, BYD 등 – 자체 설계 SoC 또는 커스텀 플랫폼.

### 4. “제품”으로 제공되는 자율주행/ADAS 솔루션 업체

여기서는 단순 센서칩이 아니라, 센서 + ECU + SW 묶음 또는 완성차에 통합된 모듈형 제품을 의미하는 쪽으로 정리해볼게요.

#### 4-1. Tier-1 ADAS 패키지

| 업체 | 주요 제공 제품/역할 | 센서 및 특징 |
| :--- | :--- | :--- |
| **Bosch** | ADAS ECU, ACC/AEB/차선유지 등 **종합 패키지** | 전방 레이더, 코너 레이더, 카메라, ADAS ECU 등 |
| **Continental** | ADAS 컨트롤 유닛 패키지 | 레이더/카메라/초음파/ADAS 컨트롤 유닛 통합 |
| **Denso** | 레이더/카메라 + ADAS ECU 공급 | 특히 **일본 OEM** (Toyota, Subaru 등)에 주력 공급 |
| **Valeo** | ADAS/자율주행 센서 패키지 | **Scala LiDAR** 포함 센서 패키지 제공 |
| **Aptiv** | ADAS/자율주행 컴퓨트 플랫폼 | 레이더/카메라 모듈 + 컴퓨트 플랫폼 |
| **ZF, Hyundai Mobis** | L2+ 시스템 및 센서 퓨전 ECU | 레이더/카메라/센서 퓨전 ECU 및 L2+ 시스템 공급 |

* 참고
   * 이들 회사는 OEM 요구에 맞춰 **"카메라 + 레이더 + 소프트웨어"**까지 통합된 패키지를 제공하며, OEM은 이를 차종별로 튜닝·검증해서 양산 적용합니다.

#### 4-2. Vision 중심/Full-stack 자율주행 플랫폼

| 업체 / 그룹 | 제품 형태 / 주요 역할 | 특징 |
| :--- | :--- | :--- |
| **Mobileye** | EyeQ 기반 ADAS부터 SuperVision, Chauffeur 같은 고급 자율주행 스택. | 다중 **카메라** + 레이더/LiDAR (옵션) + 고정밀 맵. **비전 인식**에 강점. |
| **NVIDIA, Qualcomm** | HW + SW 스택 (Perception, Planning, Control) 및 레퍼런스 아키텍처. | **고성능 컴퓨팅 플랫폼** (HW + SW)을 OEM/티어1에 공급. |
| **Huawei, Baidu, Pony.ai, Momenta 등** (주로 중국) | 고급 ADAS~L4 수준의 **Full-stack 자율주행 시스템** 패키지. | 특정 OEM에 Perception, Planning, Control을 포함한 시스템을 패키지 공급. |
| **Waymo, Cruise 등** (로보택시) | End-to-end **로보택시 플랫폼** (모빌리티 서비스용 제품). | 자체 **센서 스택** + **HD맵** + **클라우드**까지 포함. |

---

### 5. “mmWave 레이더 + 이미지 센서” 조합의 포지션 정리

| 항목 | 내용 |
| :--- | :--- |
| **산업의 기본 선택지** | **L1~L2+ 수준** (ACC, AEB, LKA, HDA 등)에서는 전방 **카메라 + 전방 장거리 레이더 + 측방/후방 레이더 + 초음파** 구성이 **가격/성능/안전성** 면에서 가장 합리적이며, 대부분의 대중차가 이 구조를 채택.  |
| **LiDAR의 역할** | 고급 옵션 또는 **L3 이상**에서 안전 여유도 확보용으로 들어가는 추세 (Volvo, Mercedes, 중국 하이엔드 EV 등). |
| **LiDAR의 현주소** | 아직은 **비용·패키징·세척 시스템** 등의 부담이 커서 전 차종 기본 구성이 되기에는 이른 상태. |
| **카메라 only vs 멀티센서** | Tesla처럼 카메라 중심(레이더 제거) 접근도 있지만, 업계 전반에서는 **카메라+레이더+기타 센서**를 함께 쓰는 **멀티센서/퓨전 구조**가 안전·규제 측면에서 더 보수적이고 현실적인 방향으로 평가됨. |



5. “mmWave 레이더 + 이미지 센서” 조합의 포지션 정리

지금 산업의 기본 선택지

L1~L2+ 수준(ACC, AEB, LKA, HDA 등)에서는
→ 전방 카메라 + 전방 장거리 레이더 + 측방/후방 레이더 + 초음파 구성이 가격/성능/안전성 면에서 가장 합리적이라서, 대부분의 대중차가 이 구조를 채택하고 있습니다.
MDPI
+1

LiDAR는?

고급 옵션 or L3 이상에서 안전 여유도 확보용으로 들어가는 추세(Volvo, Mercedes, 중국 하이엔드 EV 등).

아직은 비용·패키징·세척 시스템 등의 부담이 있어서 전 차종 기본 구성이 되기에는 이른 상태.
IDTechEx
+1

카메라 only vs 멀티센서

Tesla처럼 카메라 중심(레이다 제거) 접근도 있지만, 업계 전반에서는 카메라+레이더+기타 센서를 함께 쓰는 멀티센서/퓨전 구조가 안전·규제 측면에서 더 보수적인(=현실적인) 방향으로 평가됩니다.
MDPI
+2
ScienceDirect
+2

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
**Radar Signal Processing**
- Range FFT, Doppler FFT, Angle FFT
- CFAR Detection
- Clustering
- Radar Tracklet 생성 (object-level 추적 이전의 점 집합)

## 📌 Layer 3: Sensor Fusion & Planning Layer
**Sensor Fusion (Multiple Modalities)**
- Camera 객체 + Radar 거리/속도
- Kalman Filter / UKF 기반 Object Fusion
- Deep Fusion 기반 Feature-level Fusion
**Behavior Planning**
- ACC Distance Keeping
- Cut-in / Cut-out 판단
- Overtake / Lane-change Decision
- Collision Avoidance Behavior
**Trajectory Planning**
- Polynomial / Spline trajectory
- Speed Profile Generation
- Lateral & Longitudinal Control Path 생성

## 📌 Layer 4: Vehicle Control Layer
* Steering Control (LQR, Stanley, MPC 등)
* Brake Control
* Throttle Control
* Actuator Feedback Loop

## 📌 L2+ Full Architecture Diagram (텍스트 버전)
```
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
```

# ✅ 2. 센서 배치 설계 가이드 (FOV, Blind Spot 분석)

L2+ 수준에서는 센서 배치가 인지 성능을 크게 좌우한다.

## 📌 2.1 Front Camera 배치 규칙

* 장착 높이: 1.2–1.5 m
* Pitch 각도: -1° ~ +2° (지면을 약간 내려다보도록)
* FOV: 60°–120°
* 목표:
   * 보행자/차량 인식
   * 차선 인식
   * 신호등/표지판 인식

## 📌 2.2 Long-Range Radar (LRR) 배치
* 높이: 0.4–0.7 m
* FOV: 10°–20° (좁고 멀리 보는 구조)
* Range: 최대 250m
* 목표:
   * ACC (Adaptive Cruise Control)
   * AEB (Automatic Emergency Braking)
   * Cut-in 차량 조기 감지
* LRR은 오염(눈/진흙)에 대한 영향이 적어 범퍼 내부에 배치한다.

## 📌 2.3 Corner Radar (SRR/MRR) 배치
* FOV: 90°–150°
* Range: 20–80m
* 역할:
   * BSD (Blind Spot Detection)
   * LCA (Lane Change Assist)
   * CTA (Cross Traffic Alert)
* 차량 4모서리에 배치하여 전·후측면의 사각 영역을 제거한다.

## 📌 Blind Spot Diagram
```
             FRONT
               ^
               |
       [SRR]  Car  [SRR]
           \       /
            \     /
         Blind Spots
```

# ✅ 3. mmWave Radar 기반 Target Tracking 알고리즘 설명

* Radar Tracking은 크게 **신호 처리(Signal domain)**와 **추적(Track domain)**으로 나뉜다.

## 📌 3.1 신호 처리 단계 (Signal Processing)

* Step 1: Range FFT
  * 거리(Range) 축으로 FFT 수행하여 반사체 위치 추정.
* Step 2: Doppler FFT
  * 속도(상대 속도) 추출.
* Step 3: Angle Estimation (AoA)
  * MIMO 레이아웃 기반 멀티채널 위상차로 각도(angle) 추정.
* Step 4: Range-Doppler Map & 3D Cube 생성

## 📌 3.2 CFAR (Constant False Alarm Rate) Detection
* 노이즈 기반 threshold를 동적으로 계산하여 실제 target peak만 검출.
  * CA-CFAR
  * OS-CFAR
  * GO-CFAR

## 📌 3.3 Clustering
* 검출된 점들을 실제 object로 묶는 과정.
   * DBSCAN
   * K-means

## 📌 3.4 Tracking (추적 필터)

* Kalman Filter
  * 차량의 움직임이 선형에 가까울 때 유리
  * 도심 자동주행에서 널리 사용
* UKF / IMM (Interacting Multiple Model)
  * 곡선/고속 상황에서 더 안정적
  * 모델 전환(정속/가속/선회) 가능

## 📌 Tracking Pipeline 요약
```
ADC → FFT → CFAR → Clustering → Track Association → Kalman/UKF Tracking
```


# ✅ 4. 카메라 인지 DNN 구조 (ResNet / YOLO 기반)

# 📌 4.1 YOLO 기반 구조 설명

* YOLO는 End-to-End Real-time Object Detection 구조이다.

```
Input → Backbone(CSPDarkNet/ResNet) → Neck(PANet/FPN) → Head
```

* Backbone 역할:
  * Feature extraction
  * ResNet, CSPDarkNet53 등이 대표
* Neck 역할:
  * Multi-scale Feature Fusion
  * PANet / FPN 구조
* Head 역할:
  * Bounding Box 회귀
  * Class 예측
  * Confidence score 계산

## 📌 4.2 YOLO Example Architecture Diagram
```
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
```

## 📌 4.3 Multi-task Camera Perception (ADAS 특화)

* 카메라 1대에서 아래 기능을 동시에 수행할 수 있다:
  * Vehicle detection
  * Pedestrian detection
  * Lane detection
  * Traffic light recognition
  * Traffic sign classification
  * Drivable area segmentation

# ✅ 5. 센서 퓨전(EKF / UKF / Deep Fusion) 코드 템플릿

## 📌 5.1 EKF (Extended Kalman Filter) 기본 구조
```
def ekf_update(x, P, z, H, R):
    y = z - H @ x
    S = H @ P @ H.T + R
    K = P @ H.T @ np.linalg.inv(S)
    x = x + K @ y
    P = (np.eye(len(x)) - K @ H) @ P
    return x, P
```

## 📌 5.2 UKF (Unscented Kalman Filter) 핵심 요약

* UKF는 비선형 모델에서 더 높은 정확도를 제공.
* 절차:
  * Sigma point 생성
  * 비선형 함수에 통과
  * 예측 결과 평균/공분산 계산
  * Update 단계 수행

## 📌 5.3 Deep Fusion 구조 (Feature-level Fusion)

* 카메라 + 레이더 feature 레벨에서 딥러닝 기반으로 융합하는 방식.
```
Camera Feature → CNN → Fc_C
Radar Feature → 1D-CNN → Fc_R

Fusion:
  F = MLP([Fc_C, Fc_R])
```

* Deep Fusion은 L2+ 객체 인식 성능을 획기적으로 올리는 최신 구조이다.

# ✅ 6. ADAS 개발 로드맵 (1~3년 계획)

* ADAS 개발은 센서 인지 → Fusion → Planning → 실도로 검증 순으로 진행해야 한다.

📅 Year 1 — Perception & Fusion Foundation
  * Radar + Camera Perception 기본 구축
  * Object detection + Radar CFAR tracking
  * EKF 기반 Object-level Sensor Fusion
  * ACC / AEB / LKA 등 핵심 ADAS 기능 구현
  * HIL Simulation 구축

📅 Year 2 — System Enhancement
  * 4D Imaging Radar 적용
  * Deep Fusion 적용
  * Overtake / Cut-in / Cut-out planning 강화
  * 스플라인/최적화 기반 Trajectory Planner 도입
  * 기능 안전(ISO 26262) 대응 시작

📅 Year 3 — L2+/L3 Ready Platform
  * Redundancy architecture 설계
  * LiDAR 옵션 적용 가능 구조 확보
  * OTA + Continual Learning 체계 구축
  * 도심 환경에서의 고난도 위험 상황 대응
  * Mass-production (양산) Validation 프로세스 정립

