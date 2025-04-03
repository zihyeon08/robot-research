# 📄 연구 계획서: 3자유도 로봇팔 동역학 모델링 및 제어 최적화

## 🎯 연구 목적
- 3자유도 로봇팔의 운동을 분석하고, 라그랑주 및 해밀턴 역학 기반으로 토크를 계산하여 정밀한 동작 설계
- 수학적으로 위치 및 자세를 표현하고, 하드웨어 실험을 통해 동작을 고찰
- 수치해석 및 통계 기반 데이터 분석을 통해 최적 동작 패턴 도출

## 🧪 연구 가설
> 라그랑주 역학을 사용한 토크 계산은 뉴턴 역학 기반 제어보다 더 효율적인 제어를 가능하게 할 것이다.

## 📚 배경 이론
- **동역학 및 제어이론**: 자유도, 뉴턴/라그랑주/해밀턴 역학
- **위치 및 자세 표현**: DH 파라미터, 행렬 변환
- **데이터 분석 및 최적화**: 랜덤 포레스트, 뉴럴 네트워크

## 🔬 연구 방법 및 수행 절차

### ✅ 1단계: 로봇팔 제작
- MG90S 서보모터, 오렌지보드, 아두이노 기반 3자유도 로봇팔 조립
- 기본 동작 확인, 초기 상태 테스트

### ✅ 2단계: 위치 및 자세 모델링
- DH 파라미터 기반 정기구학 (Forward Kinematics)
- 행렬식 구성:
  ![T_i](https://latex.codecogs.com/svg.image?T_i%20%3D%20Rot_z(\theta_i)%20\cdot%20Trans_z(d_i)%20\cdot%20Trans_x(a_i)%20\cdot%20Rot_x(\alpha_i))
- 👉 참고: [계산 설명서 1단계](calculate.md)

### ✅ 3단계: 동역학 모델링
- 라그랑지안 기반 운동/위치 에너지 계산
- 토크 계산 공식:
  ![tau](https://latex.codecogs.com/svg.image?\tau_i%20%3D%20\frac{d}{dt}\left(\frac{\partial%20L}{\partial%20\dot{q}_i}\right)%20-%20\frac{\partial%20L}{\partial%20q_i})
- 👉 참고: [계산 설명서 2단계](calculate.md)

### ✅ 4단계: 실시간 상태 측정
- 각도 센서(A5600) + Python 수치 미분으로 속도/가속도 계산  
  ![q_dot](https://latex.codecogs.com/svg.image?\dot{q}%20%3D%20\frac{q(t)%20-%20q(t-\Delta%20t)}{\Delta%20t})

### ✅ 5단계: 제어기 설계 및 적용
- 토크 + PID 조합 제어기:  
  ![u](https://latex.codecogs.com/svg.image?u%20%3D%20\tau%20%2B%20K_p%20e%20%2B%20K_d%20\dot{e})

### ✅ 6단계: 데이터 분석 및 성능 비교
- RMSE, 응답시간, 토크 소비 등 비교 분석
- ML 예측 모델 도입 시도
- 👉 시뮬레이션 예시는 [예상 시뮬레이션 문서](simulation.md) 참고

## ✅ 기대 효과
- 정밀 제어 기반 로봇팔 설계 모델 검증
- 이론+실험 기반 비교 분석으로 제어 성능 향상
- 데이터 기반 피드포워드 제어기 학습 및 확장 가능성

---

## 🔁 문서 이동

- 📄 [사전 배경 지식 & 계산 입력 파라미터 정리](pre-investigation.md)
- 🧪 [연구 계획서](research-docs.md)
- 📄 [연구 절차 통합 정리 문서](solution-guide.md)
- 🧪 [계산 과정 설명서](calculate.md)
- 📄 [연구 예상 시뮬레이션](simulation.md)
