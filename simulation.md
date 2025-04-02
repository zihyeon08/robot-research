 🧪 예상 시뮬레이션 문서: 6자유도 로봇팔 제어 실험 예측

실제 연구 수행 전, 각 계산 단계를 Python 코드로 시뮬레이션하고 예상 출력을 추정합니다.

---

## ✅ 1단계: 서보모터 기본 동작 시뮬레이션

```python
import time
start = time.time()
# 예시: 서보모터 90도 → 120도 이동
time.sleep(0.45)
end = time.time()
print(f"동작 시간: {end - start:.2f}초")
```

예상 결과: 약 0.45초

## ✅ 2단계: 위치 계산 (정기구학)

```python
복사
편집
import numpy as np
def rot_z(t):
    return np.array([[np.cos(t), -np.sin(t), 0, 0],
                     [np.sin(t),  np.cos(t), 0, 0],
                     [0, 0, 1, 0],
                     [0, 0, 0, 1]])

def trans_x(a):
    return np.array([[1, 0, 0, a],
                     [0, 1, 0, 0],
                     [0, 0, 1, 0],
                     [0, 0, 0, 1]])

T = rot_z(np.radians(30)) @ trans_x(10) @ rot_z(np.radians(45)) @ trans_x(10)
print("말단 위치:", T[:3, 3])
```

예상 위치: (6.1, 23.0, 0)

## ✅ 3단계: 라그랑주 기반 토크 계산

```python
import sympy as sp
theta, dtheta, ddtheta = sp.symbols('theta dtheta ddtheta')
m, l, g = 0.5, 0.1, 9.81
I = 0.5 * m * (l**2)
T = 0.5 * I * dtheta**2
V = m * g * (l/2) * (1 - sp.cos(theta))
L = T - V
tau = sp.diff(L, dtheta).diff() * ddtheta - sp.diff(L, theta)
val = tau.evalf(subs={theta: sp.rad(30), dtheta: 0.2, ddtheta: 0.5})
print("토크 (Nm):", val)
```
예상 결과: 약 0.13 Nm

## ✅ 4단계: 속도 및 가속도 추정

```python
import numpy as np
angles = np.radians([30, 32, 35])
dt = 0.02
v1 = (angles[1] - angles[0]) / dt
v2 = (angles[2] - angles[1]) / dt
a = (v2 - v1) / dt
print(f"속도: {v2:.3f} rad/s, 가속도: {a:.3f} rad/s²")
```

예상: 속도 ≈ 0.262 rad/s, 가속도 ≈ 6.54 rad/s²

## ✅ 5단계: 제어기 출력 예측

```python
import numpy as np
theta_now = np.radians(30)
theta_ref = np.radians(40)
error = theta_ref - theta_now
d_error = 0.1
Kp, Kd = 2.0, 0.5
tau_model = 0.12
u = tau_model + Kp * error + Kd * d_error
print(f"제어 입력 u = {u:.3f} Nm")
```

예상 제어 입력: u ≈ 0.52 Nm

## ✅ 6단계: 오차 기반 분석

```python
from sklearn.metrics import mean_squared_error
actual = [29, 31, 34, 38]
target = [30, 32, 35, 40]
rmse = mean_squared_error(target, actual, squared=False)
print(f"RMSE: {rmse:.3f} deg")
```

예상 RMSE: 약 1.12°

📁 [문서 이동](./research-docs.md)
📐 [계산 설명서로](./calculate.md)
