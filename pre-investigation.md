# 📘 6자유도 로봇팔 연구: 배경지식 및 계산 정리 문서

이 문서는 6자유도 로봇팔의 동역학 모델링, 제어기 설계, 실시간 추정, 시뮬레이션을 종합적으로 수행하기 위한 **핵심 배경 이론**과 **단계별 계산 흐름**, 그리고 **필요 파라미터**를 통합 정리한 문서입니다.

---

## 🧠 1. 핵심 배경 이론 정리

### 📌 동역학 및 제어 이론
- **자유도(DoF)**: 독립적으로 제어 가능한 움직임 축의 수. 6-DoF는 3개 회전 + 3개 병진 가능
- **뉴턴 역학**: 힘-가속도 기반 물리 모델링 (F = ma)
- **라그랑주 역학**: 에너지 기반 동역학 모델링. 일반화 좌표 사용  
  ![](https://latex.codecogs.com/svg.image?L%20%3D%20T%20-%20V)  
  ![](https://latex.codecogs.com/svg.image?\tau_i%20%3D%20\frac{d}{dt}\left(\frac{\partial%20L}{\partial%20\dot{q}_i}\right)%20-%20\frac{\partial%20L}{\partial%20q_i})
- **해밀턴 역학**: 상태공간 기반 표현으로, 시간 진화 표현에 유리
- **PID 제어**:  
  ![](https://latex.codecogs.com/svg.image?u%20%3D%20\tau%20%2B%20K_p%20e%20%2B%20K_d%20\dot{e})

### 📌 위치 및 자세 수학 표현
- **DH 파라미터**: (θ, d, a, α) 조인트 간 위치/자세 정의
- **변환 행렬**:  
  ![](https://latex.codecogs.com/svg.image?T_i%20%3D%20R_z(\theta_i)%20\cdot%20T_z(d_i)%20\cdot%20T_x(a_i)%20\cdot%20R_x(\alpha_i))

### 📌 데이터 분석
- **성능 지표**: RMSE, 토크 소비량, 응답 시간 등
- **ML 모델**: 랜덤포레스트, 신경망 → 토크 예측, 궤적 보정

---

## 🧮 2. 단계별 계산 흐름 및 수식 요약

| 단계 | 계산 대상 | 주요 수식 | 입력값 | 출력값 | 구현 도구 |
|------|------------|------------|---------|----------|------------|
| 1단계 | 정기구학 | ![](https://latex.codecogs.com/svg.image?T_i%20%3D%20R_z(\theta_i)T_z(d_i)T_x(a_i)R_x(\alpha_i)) | DH 파라미터 | 말단 위치/자세 | Python (numpy, sympy) |
| 2단계 | 동역학 모델링 | ![](https://latex.codecogs.com/svg.image?L%20%3D%20T%20-%20V), ![](https://latex.codecogs.com/svg.image?\tau%20=%20\frac{d}{dt}(\frac{\partial%20L}{\partial%20\dot{q}})-\frac{\partial%20L}{\partial%20q}) | m, I, q, 𝑞̇, 𝑞̈ | 관절 토크 | Python (sympy) |
| 3단계 | 상태 추정 | ![](https://latex.codecogs.com/svg.image?\dot{q}_i%20=%20\frac{q(t)-q(t-\Delta%20t)}{\Delta%20t}), ![](https://latex.codecogs.com/svg.image?\ddot{q}_i%20=%20\frac{\dot{q}(t)-\dot{q}(t-\Delta%20t)}{\Delta%20t}) | 센서 측정값 | 𝑞̇, 𝑞̈ | Arduino + Python |
| 4단계 | 제어 입력 | ![](https://latex.codecogs.com/svg.image?u%20=%20\tau%20+%20K_p%20e%20+%20K_d%20\dot{e}) | τ, e, ė | PWM 제어 입력 | Python + Arduino |
| 5단계 | 성능 분석 | ![](https://latex.codecogs.com/svg.image?RMSE%20=%20\sqrt{\frac{1}{n}%20\sum(q_{target}-q_{actual})^2}) | 실험 데이터 | RMSE 등 지표 | pandas, scikit-learn |

---

## 🔧 3. 파라미터 정의 및 예시값

### 📍 DH 파라미터 예시 (1~2축)

| 축 | θ_i | d_i | a_i | α_i |
|----|-----|-----|-----|------|
| 1  | θ₁  | 0   | 10cm| 90° (π/2) |
| 2  | θ₂  | 0   | 10cm| 0°       |

### 📍 물리 파라미터 예시

| 항목 | 값         |
|------|------------|
| 링크 길이 (l) | 0.1 m |
| 질량 (m)       | 0.5 kg |
| 중력 (g)       | 9.81 m/s² |
| 관성 (I)       | 0.5 * m * l² |

### 📍 제어기 파라미터 예시

| 항목        | 값      |
|-------------|----------|
| Kp          | 2.0      |
| Kd          | 0.5      |
| 모델 토크 τ | 0.12 Nm  |

---

## 🔬 4. 시뮬레이션 기반 예측 결과 요약

| 항목                 | 예상 값               | 설명 |
|----------------------|------------------------|------|
| 서보모터 동작 시간   | 약 0.45초             | 90° → 120° 이동 |
| 말단 위치 계산       | (6.1, 23.0, 0)         | DH 기준 변환 |
| 토크 계산            | 약 0.13 Nm            | 단일 링크, 라그랑주 |
| 속도/가속도          | 0.26 rad/s, 6.54 rad/s² | 센서 기반 수치 미분 |
| 제어 입력 (u)        | 약 0.52 Nm            | PID + 모델기반 |
| RMSE                 | 약 1.12°              | 목표 vs 실제 |

---

## 📌 참고 문서

- `research-docs.md` - 연구 계획서  
- `calculate.md` - 계산 설명서  
- `simulation.md` - 시뮬레이션 예시  
