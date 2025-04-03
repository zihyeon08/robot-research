# 📊 3자유도 로봇팔 연구 절차 통합 정리

이 문서는 3자유도 로봇팔의 동역학 모델링 및 제어 최적화 연구를 효과적으로 수행하기 위한 전체 흐름을 순차적으로 설명합니다. 각 단계는 수학적 모델링, 시뮬레이션, 하드웨어 적용, 데이터 분석을 포함하여 실제 구현 가능한 연구 계획으로 구성되어 있습니다.

---

## 🔁 전체 연구 흐름 요약

```
로봇팔 조립 완료
       ↓
1. 기구학 모델링 (DH 파라미터 → Forward Kinematics)
       ↓
2. 동역학 모델링 (라그랑주 → 토크 계산)
       ↓
3. 실시간 센싱 및 상태 추정 (q, q_dot, q_ddot)
       ↓
4. 제어기 설계 (토크 + PID)
       ↓
5. 시뮬레이션 → 하드웨어 테스트
       ↓
6. 데이터 수집 및 분석 (RMSE, ML 적용)
       ↓
7. 최적 제어 설계 및 결과 정리
```

---

## 📌 단계별 설명

### 1. 기구학 모델링 (정기구학)
- **목표**: 관절 각도 → 말단 위치/자세 계산
- **도구**: DH 파라미터 (3축), 변환 행렬, Python (`NumPy`, `SymPy`)
- **산출물**: `forward_kinematics()` 함수, T₀³ 말단 위치

---

### 2. 동역학 모델링 (Lagrangian 기반)

- **목표**: 각 관절에 필요한 토크 계산  
- **수식**:
  - 운동에너지 T, 위치에너지 V  
    ![L = T - V](https://latex.codecogs.com/svg.image?L%20%3D%20T%20-%20V)
  - 라그랑주 방정식  
    ![tau](https://latex.codecogs.com/svg.image?\tau%20%3D%20\frac{d}{dt}\left(\frac{\partial%20L}{\partial%20\dot{q}}\right)%20-%20\frac{\partial%20L}{\partial%20q})
- **도구**: Python (`SymPy`), 질량/관성/링크 길이 정보

---

### 3. 실시간 상태 추정

- **목표**: q, 𝑞̇, 𝑞̈ 계산  
- **도구**: 각도 센서 (A5600), Python (수치 미분)  
- **수식**:  
  - 속도  
    ![q_dot](https://latex.codecogs.com/svg.image?\dot{q}_i%20%3D%20\frac{q_i(t)%20-%20q_i(t-\Delta%20t)}{\Delta%20t})
  - 가속도  
    ![q_ddot](https://latex.codecogs.com/svg.image?\ddot{q}_i%20%3D%20\frac{\dot{q}_i(t)%20-%20\dot{q}_i(t-\Delta%20t)}{\Delta%20t})

---

### 4. 제어기 설계 및 시뮬레이션
- **목표**: PID + 토크 기반 제어기 설계
- **제어식**:  
  ![control_eq](https://latex.codecogs.com/svg.image?u%20%3D%20\tau%20%2B%20K_p%20e%20%2B%20K_d%20\dot{e})
- **도구**: Python, Arduino, 실시간 제어 테스트
- **성과**: 목표 위치에 안정적 수렴

---

### 5. 시뮬레이션 및 실험 연동
- **목표**: 제어기 → 실제 하드웨어 연동
- **방법**: Python 제어 신호 → Arduino PWM 전송
- **실험 항목**: 응답 시간, 궤적 정확도, 동작 반복성 등

---

### 6. 데이터 기반 분석 및 최적화
- **목표**: 성능 분석 및 머신러닝 기반 보정
- **지표**: RMSE, 토크 소비, 응답 시간
- **도구**: `pandas`, `scikit-learn`, `PyTorch`
- **활용**: 토크 예측, 비선형 보정, 경로 학습 등

---

### 7. 결과 정리 및 논문화
- **내용**:
  - 모델 및 제어 수식 정리
  - 실험 결과 시각화
  - 라그랑주 기반 제어의 성능 평가
- **산출물**: 보고서, 논문, 발표 자료 등

---

## 🔁 문서 이동

- 📄 [사전 배경 지식 & 계산 입력 파라미터 정리](pre-investigation.md)
- 🧪 [연구 계획서](research-docs.md)
- 📄 [연구 절차 통합 정리 문서](solution-guide.md)
- 🧪 [계산 과정 설명서](calculate.md)
- 📄 [연구 예상 시뮬레이션](simulation.md)

