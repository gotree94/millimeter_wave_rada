# 🛰️ mmWave Radar Sensor Solutions Guide

밀리미터파(mmWave) 레이더 센서 솔루션 종합 가이드입니다.  
자율주행, ADAS, IoT, 스마트홈 등 다양한 애플리케이션을 위한 하드웨어 및 소프트웨어 솔루션을 정리했습니다.

---

## 📋 목차

- [개요](#-개요)
- [벤더별 솔루션](#-벤더별-솔루션)
  - [Texas Instruments (TI)](#1-texas-instruments-ti)
  - [Infineon Technologies](#2-infineon-technologies)
  - [NXP Semiconductors](#3-nxp-semiconductors)
  - [Acconeer](#4-acconeer)
  - [Analog Devices (ADI)](#5-analog-devices-adi)
  - [STMicroelectronics](#6-stmicroelectronics)
  - [Hi-Link](#7-hi-link)
- [애플리케이션별 추천](#-애플리케이션별-추천)
- [비교표](#-벤더별-비교표)
- [참고 자료](#-참고-자료)

---

## 📡 개요

### 밀리미터파 레이더란?

밀리미터파 레이더는 24GHz ~ 81GHz 주파수 대역을 사용하여 물체의 거리, 속도, 각도를 감지하는 센서입니다.

| 주파수 대역 | 특징 | 주요 용도 |
|-------------|------|-----------|
| **24GHz** | 긴 감지거리, 저비용 | BSD, 스마트홈, 산업용 |
| **60GHz** | 고해상도, 소형화 | 실내감지, CPD, 제스처 |
| **77GHz** | 넓은 대역폭, 고정밀 | 자율주행, ADAS LRR |

### 주요 변조 방식

- **FMCW (Frequency Modulated Continuous Wave)**: 대부분의 자동차/산업용 레이더
- **PCR (Pulsed Coherent Radar)**: Acconeer 고유 기술, 초저전력

---

## 🏭 벤더별 솔루션

### 1. Texas Instruments (TI)

> 가장 포괄적인 SDK와 개발 생태계를 보유한 업계 선두 벤더

#### 하드웨어 제품군

| 제품명 | 주파수 | 칩셋 | 안테나 | 프로세서 | 타겟 시장 |
|--------|--------|------|--------|----------|-----------|
| **AWR1642BOOST** | 76-81GHz | AWR1642 | 외장 PCB | C674x DSP + ARM R4F | Automotive |
| **AWR6843ISK** | 60-64GHz | AWR6843 | 외장 Long-range | C674x DSP + ARM R4F | Automotive |
| **AWR6843AOPEVM** | 60-64GHz | AWR6843AOP | AoP | C674x DSP + ARM R4F | Automotive |
| **AWRL6844EVM** | 57-64GHz | AWRL6844 | 내장 | Edge AI HW Accel + DSP | Automotive (저전력) |
| **IWRL6432BOOST** | 57-64GHz | IWRL6432 | FR4 PCB | ARM Cortex-M4F | Industrial |
| **IWR6843ISK** | 60-64GHz | IWR6843 | 외장 Long-range | C674x DSP + ARM R4F | Industrial |

#### 소프트웨어 생태계

| SDK/도구 | 지원 보드 | 주요 기능 |
|----------|-----------|-----------|
| **MMWAVE-SDK** | AWR1642, AWR6843, IWR6843 | 레이더 신호처리, Out-of-box Demo |
| **MMWAVE-L-SDK** | IWRL6432, AWRL6844 | 저전력 모드, Motion/Presence Detection |
| **mmWave Studio GUI** | 전체 | RF 특성 평가, ADC 데이터 캡처 |
| **mmWave Demo Visualizer** | 전체 | Point Cloud 시각화 |
| **DCA1000EVM** | 전체 (옵션) | Raw ADC 데이터 스트리밍 |

**서드파티 지원:**
- [MATLAB Radar Toolbox](https://www.mathworks.com/help/radar/ug/getting-started-timmwaveradar-example.html)
- [ti_mmwave_rospkg (ROS)](https://github.com/radar-lab/ti_mmwave_rospkg)
- [pymmw (Python)](https://github.com/m6c7l/pymmw)

#### 링크
- 🔗 [TI mmWave SDK](https://www.ti.com/tool/MMWAVE-SDK)
- 🔗 [TI Radar Academy](https://www.ti.com/design-development/embedded-development/mmwave-radar.html)

---

### 2. Infineon Technologies

> 77GHz 자동차 레이더 시장 점유율 1위, 4D 이미징 레이더까지 확장 가능

#### 하드웨어 제품군

| 제품군 | 주파수 | 주요 제품 | 개발 키트 | 타겟 시장 |
|--------|--------|-----------|-----------|-----------|
| **RASIC™** | 77/79GHz | RXS8160PL (3T4R) | CARKIT 2C3 | Automotive ADAS |
| **XENSIV™ 60GHz** | 60GHz | BGT60TR13C (1T3R, AiP) | DEMO-BGT60TR13C | Industrial/IoT |
| **XENSIV™ 24GHz** | 24GHz | BGT24LTR11 | SENSE2GOL PULSE | Industrial |

#### 소프트웨어 생태계

| 도구 | 설명 |
|------|------|
| **Radar Development Kit (RDK)** | C/C++, Python, MATLAB API 지원 |
| **Radar Fusion GUI** | 실시간 시각화, 알고리즘 테스트 |
| **ModusToolbox™** | PSoC 기반 통합 개발환경 |

**MCU 연동:**
- AURIX™ TC3xx (ISO 26262 ASIL-D)
- PSoC™ 6 시리즈

#### 링크
- 🔗 [Infineon Radar Sensors](https://www.infineon.com/products/sensor/radar-sensors)
- 🔗 [Radar Development Kit](https://www.infineon.com/design-resources/development-tools/sdk/radar-development-kit)

---

### 3. NXP Semiconductors

> 분산형(Distributed) 레이더 아키텍처 지원, Tier-1과의 협력 생태계 강점

#### 하드웨어 제품군

| 제품군 | 주파수 | 주요 제품 | 개발 키트 | 타겟 시장 |
|--------|--------|-----------|-----------|-----------|
| **SAF85xx** | 77GHz | SAF85xx SoC (4T4R) | SAF85xx EVK | Automotive |
| **TEF810x** | 77GHz | TEF8102 Transceiver | - | Automotive |
| **S32R** | - | S32R27, S32R37 Processor | RDK-S32R27 | 신호처리 MCU |

#### 소프트웨어 생태계

| 도구 | 설명 |
|------|------|
| **Automotive Radar SDK** | ACC, AEB 알고리즘 포함 |
| **SPT (Signal Processing Toolbox)** | 레이더 신호처리 전용 |
| **S32 Design Studio** | 통합 개발환경 |

#### 링크
- 🔗 [NXP Automotive Radar](https://www.nxp.com/applications/automotive/adas-and-safe-driving/automotive-radar-systems:RADAR-SYSTEMS)

---

### 4. Acconeer

> 스웨덴 기업, 초저전력 PCR 기술로 배터리 구동 IoT에 최적화

#### 하드웨어 제품군

| 제품군 | 주파수 | 주요 제품 | 개발 키트 | 특징 |
|--------|--------|-----------|-----------|------|
| **A1 Family** | 60GHz | A121 (PCR, AiP) | XE121 + XC120 | 29mm², AEC-Q100 Grade 2 |
| **Modules** | 60GHz | XM125 (Entry+) | XE125 EVK | LGA 패키지, 1.8V 동작 |

#### 소프트웨어 생태계

| 도구 | 설명 |
|------|------|
| **Exploration Tool** | Python 기반 GUI |
| **RSS (Radar System Software)** | 거리/모션/존재감지 알고리즘 |

**플랫폼 호환:**
- Raspberry Pi
- STM32 Nucleo

#### 링크
- 🔗 [Acconeer Products](https://acconeer.com/products/)

---

### 5. Analog Devices (ADI)

> 24GHz 칩셋 분리형 설계로 유연성 높음, 디지털 빔포밍(DBF) 지원

#### 하드웨어 제품군

| 제품군 | 주파수 | 주요 제품 | 개발 키트 | 타겟 시장 |
|--------|--------|-----------|-----------|-----------|
| **MMIC Chipset** | 24GHz | ADF5901 (Tx), ADF5904 (4ch Rx) | EV-RADAR-MMIC2 | Industrial |
| **Demo Platform** | 24GHz | TINYRAD | EV-TINYRAD24G | Prototyping |
| **ADC** | - | ADAR7251 | ADAR7251 Mini Board | 신호처리 |

#### 소프트웨어 생태계

| 도구 | 설명 |
|------|------|
| **DEMORAD Platform** | Out-of-box 데모 (Range-Doppler, MIMO) |
| **EV-RADAR-MMIC Software** | Windows GUI |
| **MATLAB/Python 코드** | 알고리즘 예제 제공 |

#### 링크
- 🔗 [ADI Radar Systems](https://www.analog.com/en/applications/technology/radar-systems.html)

---

### 6. STMicroelectronics

> 24GHz 3500만개+ 출하 실적, Vision IC와 센서 퓨전 솔루션

#### 하드웨어 제품군

| 제품군 | 주파수 | 주요 제품 | 타겟 시장 |
|--------|--------|-----------|-----------|
| **STRADA** | 24GHz | STRADA431 | Automotive SRR |
| **77GHz IC** | 77GHz | 3T4R Transceiver | Automotive LRR |

> ⚠️ 개발 키트는 Tier-1 대상으로 제한적 공급

#### 링크
- 🔗 [ST Automotive Radar](https://www.st.com/en/applications/adas/automotive-radar.html)

---

### 7. Hi-Link

> 중국 기업, 초저가 스마트홈/IoT 센서

#### 하드웨어 제품군

| 제품명 | 주파수 | 감지거리 | 인터페이스 | 특징 |
|--------|--------|----------|------------|------|
| **LD2410** | 24GHz | 5m | GPIO + UART | 인체 존재감지 |
| **LD2411S** | 24GHz | 6m (동적), 3.5m (정적) | UART + BLE | 거리측정, 앱설정 |
| **LD2420** | 24GHz | 8m | GPIO + UART | 고감도, 플러그앤플레이 |
| **LD2450** | 24GHz | - | UART | 다중 타겟 추적 |

#### 소프트웨어

| 도구 | 설명 |
|------|------|
| **HLKRadarTools App** | Android/iOS, BLE 설정 |
| **ESPHome 통합** | Home Assistant 연동 |
| **PC 상위기** | UART 파라미터 설정 |

#### 링크
- 🔗 [Hi-Link Official](https://www.hlktech.net/)
- 🔗 [ESPHome LD2420](https://esphome.io/components/sensor/ld2420/)

---

## 🎯 애플리케이션별 추천

### 자동차 자율주행 / ADAS

| 애플리케이션 | 추천 솔루션 | 이유 |
|--------------|-------------|------|
| **장거리 레이더 (ACC, AEB)** | TI AWR1642BOOST / Infineon CARKIT 2C3 | 76-81GHz, 200m+ 검출, ISO 26262 |
| **코너 레이더 (BSD, LCA)** | TI AWR6843ISK / Infineon RASIC 77GHz | 넓은 FOV, 높은 각도 분해능 |
| **주차 보조** | TI AWR6843AOPEVM | AoP로 소형화, 근거리 고정밀 |
| **카메라+레이더 퓨전** | TI AWR6843ISK + ROS | Point Cloud + 영상 센서 퓨전 |

### 차량 실내 감지 (In-Cabin)

| 애플리케이션 | 추천 솔루션 | 이유 |
|--------------|-------------|------|
| **Child Presence Detection** | TI AWRL6844EVM / Infineon BGT60TR13C | Edge AI, 저전력, Euro NCAP 대응 |
| **Seat Belt Reminder** | TI AWRL6844EVM | 승객 감지 + 위치 파악 |
| **Driver Monitoring (DMS)** | TI AWR6843AOPEVM | Vital Signs 감지 (심박, 호흡) |

### 산업용 / 스마트 빌딩

| 애플리케이션 | 추천 솔루션 | 이유 |
|--------------|-------------|------|
| **People Counting** | TI IWRL6432BOOST / Acconeer XE125 | 저전력, Point Cloud |
| **실내 존재 감지** | Hi-Link LD2420 / Infineon BGT60TR13C | 저가 또는 고정밀 선택 |
| **거리 측정** | Acconeer A121 / Hi-Link LD2411S | 밀리미터 정밀도 |
| **스마트 조명** | Hi-Link LD2420 | GPIO 직접 제어 |

### 프로토타이핑 / 학습

| 용도 | 추천 솔루션 | 이유 |
|------|-------------|------|
| **레이더 원리 학습** | ADI EV-TINYRAD24G | 저렴, MATLAB/Python 예제 |
| **ROS 연동 개발** | TI IWR6843ISK | ROS 패키지 지원 |
| **스마트홈 DIY** | Hi-Link LD2410/LD2420 | $5~15, ESPHome 통합 |

---

## 📊 벤더별 비교표

| 벤더 | 주파수 | 자율주행 적합성 | 개발 난이도 | EVK 가격 | SDK 완성도 |
|------|--------|-----------------|-------------|----------|------------|
| **TI** | 60/77GHz | ✅ 최적 | 중~상 | $150~300 | ⭐⭐⭐⭐⭐ |
| **Infineon** | 24/60/77GHz | ✅ 최적 | 중~상 | $100~500 | ⭐⭐⭐⭐⭐ |
| **NXP** | 77GHz | ✅ 최적 | 상 | $300~500 | ⭐⭐⭐⭐ |
| **Acconeer** | 60GHz | △ 보조용 | 중 | $100~150 | ⭐⭐⭐⭐ |
| **ADI** | 24GHz | △ 보조용 | 중~상 | $200~400 | ⭐⭐⭐ |
| **ST** | 24/77GHz | ✅ 최적 | 상 | Tier-1 직접 | ⭐⭐⭐ |
| **Hi-Link** | 24GHz | ❌ | 하 | $5~15 | ⭐⭐ |

---

## 📚 참고 자료

### 공식 문서
- [TI mmWave Sensor Design](https://www.ti.com/design-development/embedded-development/mmwave-radar.html)
- [Infineon Radar Academy](https://www.infineon.com/cms/en/applications/automotive/chassis-safety-and-adas/automotive-77-ghz-radar-system/)
- [NXP Radar Solutions](https://www.nxp.com/applications/automotive/adas-and-safe-driving/automotive-radar-systems:RADAR-SYSTEMS)
- [Acconeer Documentation](https://docs.acconeer.com/)

### 커뮤니티 & 오픈소스
- [TI E2E Forum - Sensors](https://e2e.ti.com/support/sensors-group/sensors/f/sensors-forum)
- [Infineon Developer Community](https://community.infineon.com/)
- [Home Assistant - mmWave](https://community.home-assistant.io/t/ld2410c-vs-2410b-vs-ld2410s-vs-2411-vs-hlk-ld2420-vs-ld2450/652599)

### 학술 자료
- IEEE Transactions on Microwave Theory and Techniques
- SAE International - Automotive Radar Standards

---

## 📝 라이선스

이 문서는 정보 제공 목적으로 작성되었습니다.  
각 제품의 상세 스펙과 가격은 공식 웹사이트를 참조하세요.

---

## 🤝 기여

Pull Request와 Issue를 환영합니다!

- 새로운 벤더/제품 정보 추가
- 오류 수정
- 애플리케이션 사례 공유

---

*Last Updated: 2024.12*
