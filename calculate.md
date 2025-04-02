# 📐 6자유도 로봇팔 연구 계산 설명서

이 문서는 6자유도 로봇팔 동역학 모델링 및 제어 최적화 연구에서 사용되는 **계산 항목, 수식, 구현 방식**을 정리한 문서입니다.

---

## ✅ 전체 계산 흐름 요약

| 단계 | 계산 대상       | 입력 값                     | 사용 수식 / 알고리즘                               | 출력 / 목적             |
|------|------------------|-----------------------------|----------------------------------------------------|--------------------------|
| 1단계 | 위치 모델링     | DH 파라미터                 | 변환행렬 T_i, FK                                   | 말단 위치/자세          |
| 2단계 | 토크 모델링     | 질량, 관성, 위치            | 라그랑주 방정식                                     | 각 관절 토크             |
| 3단계 | 상태 측정       | 관절 각도 q_i               | 수치 미분                                           | 속도/가속도              |
| 4단계 | 제어 적용       | 토크 공식, 궤적             | 피드포워드 + PID 제어                               | 입력 신호                |
| 5단계 | 성능 최적화     | 센서 데이터                 | 오차 계산, ML 모델링                                | 제어 성능 개선 및 평가   |

---

## 🟩 1단계: 위치 및 자세 계산 (Forward Kinematics)

- **입력값**: DH 파라미터 (θ, d, a, α)
- **수식**:  
  ![T_i](https://latex.codecogs.com/svg.image?T_i%20%3D%20Rot_z(\theta_i)%20%5Ccdot%20Trans_z(d_i)%20%5Ccdot%20Trans_x(a_i)%20%5Ccdot%20Rot_x(\alpha_i))
- **출력**: 말단 위치, 자세 (T₀⁶)
- **도구**: Python (SymPy, NumPy)

---

## 🟩 2단계: 동역학 모델링 (Lagrangian 기반)

- **입력값**: 각 링크의 질량 mᵢ, 속도 𝑞̇ᵢ, 가속도 𝑞̈ᵢ, 위치
- **수식**:  
  ![L = T - V](https://latex.codecogs.com/svg.image?L%20%3D%20T%20-%20V)  
  ![tau_i](https://latex.codecogs.com/svg.image?\tau_i%20%3D%20\frac{d}{dt}\left(\frac{\partial%20L}{\partial%20\dot{q}_i}\right)%20-%20\frac{\partial%20L}{\partial%20q_i})
- **출력**: 관절별 토크
- **도구**: Python (SymPy)

---

## 🟩 3단계: 상태 측정

- **입력값**: 센서로 측정한 각도 qᵢ(t)
- **수식**:  
  ![q_dot](https://latex.codecogs.com/svg.image?\dot{q}_i%20%3D%20\frac{q_i(t)%20-%20q_i(t-\Delta%20t)}{\Delta%20t})  
  ![q_ddot](https://latex.codecogs.com/svg.image?\ddot{q}_i%20%3D%20\frac{\dot{q}_i(t)%20-%20\dot{q}_i(t-\Delta%20t)}{\Delta%20t})
- **출력**: 각속도, 각가속도
- **도구**: Arduino, Python

---

## 🟩 4단계: 제어기 적용

- **입력값**: 계산된 토크, 목표 궤적
- **수식**:  
  ![control eq](https://latex.codecogs.com/svg.image?u%20%3D%20\tau%20+%20K_p%20e%20+%20K_d%20\dot{e})
- **출력**: 제어 입력 (PWM 등)
- **도구**: Python (실시간 제어)

---

## 🟩 5단계: 데이터 기반 분석 및 최적화

- **입력값**: 실험 데이터 (시간, 각도, 오차 등)
- **분석**:
  - 성능 지표: RMSE, 응답 시간, 에너지 소비량
  - ML 모델: 랜덤포레스트, 신경망
- **출력**: 최적 제어법, 가설 검증
- **도구**: pandas, scikit-learn, PyTorch

---

📁 돌아가기: [연구 계획서로 이동](README_Lagrange_RobotArm.md)
