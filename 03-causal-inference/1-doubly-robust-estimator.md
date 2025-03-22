# Doubly Robust Estimator

Doubly-Robust Estimator에 대한 정의에 앞서, 몇 가지 함수 및 추정치를 정의해보겠습니다. 종속 변수는 y, 이진 처치 변수는 D, 그리고 nuisance variables는 X로 표기합니다. 우리는 D=0인 Control Group 내부에서 모든 변수(X와 D=0)로 y를 예측하는 모델을 만들 수 있습니다. D = 1에 대해서도 마찬가지로 모델을 만들 수 있겠죠. 만약 두 그룹이 비교 가능하다면 이것만으로도 제법 괜찮은 모델일 수도 있겠네요. (물론 일부 Learner의 경우, 수많은 공변량에서 차원의 저주에 의해 D가 0이든 1이든 효과가 고려되지 않고 X에만 의존하여 예측치를 만들어 실제 효과가 있는 정책임에도 underestimate될 확률이 있을 수 있겠습니다.)

\begin{align}
g_0(x) := E[Y\mid D=0, X=x]\\
g_1(x) := E[Y\mid D=1, X=x]\\
g_{D_i}(x) := g_1(x) * D + g_0(x) * (1-D)\\
\end{align}


또 성향점수 모델 p(x)도 아래와 같이 만들 수 있습니다. q(x)의 경우 D 변수를 사용하지 않고 Y를 예측하는 모델입니다. (DML이나 Causal Forest에서 활용되는 잔차화 방식을 위해 쓰입니다.)

\begin{align}
p(x) := E[D\mid X=x] = \Pr(D=1\mid X=x)\\
q(x) := E[Y\mid X=x]\\
\end{align}

참고로 실제로 구현할 때는 여기서 설명한 모든 추정치를 얻기 위해서 원하는 머신러닝을 골라서 사용할 수 있습니다. 단, 오버피팅 문제로 이런 방법을 쓸 때는 반드시 cross-validation을 하는 것이 하나의 규칙입니다. 구현 및 응용 방법에 대한 자세한 내용은 [링크](/CATE-inference)를 참고해주세요.

Doubly Robust라고 부르는 이유는, 성향 점수 추정치 {math}`\hat{p}(X_i)`가 부정확하더라도 {math}`\hat{g}_1(X_i)`와 {math}`\hat{g}_0(X_i)`만 정확하면 기댓값이 곧 올바른 ATE의 추정치가 되고, 반대로 {math}`\hat{g}_1(X_i)`와 {math}`\hat{g}_0(X_i)`가 부정확하더라도 성향 점수 추정치 {math}`\hat{p}(X_i)`가 정확하면 마찬가지로 기댓값이 곧 올바른 ATE의 추정치가 되기 때문입니다.

Doubly Robust Estimator는 다음과 같이 정의됩니다.

```{math}
:label: first
Y_i^{DR}(\hat{g},\hat{p}) := \hat{g}_1(X_i) - \hat{g}_0(X_i) + (Y_i - \hat{g}_{D_i}(X_i))\frac{D_i - \hat{p}(X_i)}{\hat{p}(X_i) (1-\hat{p}(X_i))}
```

이후 다음과 같이 활용될 수 있습니다. 여기서는 ATE만 증명하지만, 동일하게 X가 주어진 상황으로 정리하면 CATE로 활용할 수 있다는 사실도 똑같이 보일 수 있습니다.

```{math}
ATE = E_n\left[Y^{DR}(\hat{g},\hat{p})\right], \quad CATE = \tau(X) = E[Y^{DR}(\hat{g}, \hat{p})|X]
```

---

## Doubly Robust Estimator의 기댓값의 특성 증명

### **1. 기댓값 취하기**
Estimator에 기댓값을 취하면,

```{math}
:label: expectation
E[Y^{DR}(\hat{g},\hat{p})] = E\left[\hat{g}_1(X) - \hat{g}_0(X) + (Y - \hat{g}_{D}(X))\frac{D - \hat{p}(X)}{\hat{p}(X)(1-\hat{p}(X))} \right]
```

---

### **2. 분수 항 분리**

Doubly Robust Estimator의 핵심 항인  
```{math}
:label: core
(Y - \hat{g}_D(X)) \frac{D - \hat{p}(X)}{\hat{p}(X)(1 - \hat{p}(X))}
```
을 분해하여, 처치 그룹 ({math}`D = 1`)과 통제 그룹 ({math}`D = 0`)으로 나누어 전개합니다.

---

#### **2-1. 분해를 위한 트릭**
먼저, {math}` D - \hat{p}(X) `에 {math}`D \hat{p}(X)`를 빼고 더하는 트릭이 필요합니다.

```{math}
D - \hat{p}(X) = D - D \hat{p}(X) + D \hat{p}(X) - \hat{p}(X)
```

즉, 이를 정리하면,

```{math}
D - \hat{p}(X) = D(1 - \hat{p}(X)) - (1-D) \hat{p}(X)
```

이를 원래 식에 대입하면,

```{math}
(Y - \hat{g}_D(X)) \frac{D(1 - \hat{p}(X)) - (1-D)\hat{p}(X)}{\hat{p}(X)(1 - \hat{p}(X))}
```

이를 두 개의 항으로 나누어 정리하면,

```{math}
:label: decomposed
(Y - \hat{g}_D(X)) \left( \frac{D(1 - \hat{p}(X))}{\hat{p}(X)(1 - \hat{p}(X))} - \frac{(1-D)\hat{p}(X)}{\hat{p}(X)(1 - \hat{p}(X))} \right)
```

즉, **처치 그룹 ({math}`D=1`)과 통제 그룹 ({math}`D=0`)의 항을 분리**할 수 있습니다.

---

#### **2-2. 처치 그룹 ({math}`D = 1`) 일 경우**
처치 그룹에서는 {math}`D = 1`이므로,

```{math}
(Y - \hat{g}_1(X)) \left( \frac{1 - \hat{p}(X)}{\hat{p}(X)(1 - \hat{p}(X))} - 0 \right)
```

```{math}
= (Y - \hat{g}_1(X)) \frac{1}{\hat{p}(X)}
```

---

#### **2-3. 통제 그룹 ({math}`D = 0`) 부분**
통제 그룹에서는 {math}`D = 0`이므로,

```{math}
(Y - \hat{g}_0(X)) \left( 0 - \frac{\hat{p}(X)}{\hat{p}(X)(1 - \hat{p}(X))} \right)
```

```{math}
= -(Y - \hat{g}_0(X)) \frac{1}{1 - \hat{p}(X)}
```

---

#### **2-4. 정리**
핵심 항인 [](#core)는 [](#decomposed)를 거쳐, 이제 다음과 같이 정리됩니다.

```{math}
:label: decomposed-2
(Y - \hat{g}_D(X)) \frac{D - \hat{p}(X)}{\hat{p}(X)(1 - \hat{p}(X))} = D*[(Y - \hat{g}_1(X)) \frac{1}{\hat{p}(X)}] - (1-D)*[(Y - \hat{g}_0(X)) \frac{1}{1 - \hat{p}(X)}]
```

---

### **3. 전체 항 정리**
기댓값 공식[](#expectaion)에 [](#decomposed-2)을 대입하면 다음과 같습니다.
```{math}
E[Y^{DR}(\hat{g},\hat{p})] = E\left[\hat{g}_1(X) - \hat{g}_0(X) + D*[(Y - \hat{g}_1(X)) \frac{1}{\hat{p}(X)}] - (1-D)*[(Y - \hat{g}_0(X)) \frac{1}{1 - \hat{p}(X)}] \right]
```

이는 크게 두 부분으로 나누면 다음과 같습니다.
```{math}
E[Y^{DR}(\hat{g},\hat{p})] = E\left[\hat{g}_1(X) + D*[(Y - \hat{g}_1(X)) \frac{1}{\hat{p}(X)}] \right]- E\left[\hat{g}_0(X) + (1-D)*[(Y - \hat{g}_0(X)) \frac{1}{1 - \hat{p}(X)}] \right]
```

또, 내부에서 g 관련 항끼리 묶으면 다음과 같이 정리됩니다.
```{math}
:label: decomposed-expectation
E[Y^{DR}(\hat{g},\hat{p})] = E\left[\frac{DY}{\hat{p}(X)} - \left( \frac{D - \hat{p}(X)}{\hat{p}(X)}\right) \hat{g}_1(X)\right] - E\left[\frac{(1-D)Y}{1-\hat{p}(X)} - \left( \frac{(1-D) - (1-\hat{p}(X))}{1-\hat{p}(X)}\right) \hat{g}_0(X) \right]
```

---

### **4. 왜 둘 중 하나만 정확해도 정확한 ATE 추정이 가능한가?**
#### 가정 1. 성향 점수 {math}`\hat{p}(X_i)`는 부정확하지만, {math}`\hat{g}_1(X_i)`와 {math}`\hat{g}_0(X_i)`는 정확한 경우
[](#expectation)에서 {math}`(Y - \hat{g}_{D}(X))`가 항상 0이 되므로, 기댓값에서 아래와 같이 2개 항 빼고는 사라지면서 자연스럽게 ATE 추정치임을 확인할 수 있습니다.
```{math}
:label: applied
E[Y^{DR}(\hat{g},\hat{p})] = E\left[\hat{g}_1(X) - \hat{g}_0(X) \right] = E[Y_1] - E[Y_0] = ATE
```

<br/><br/>
#### 가정 2. {math}`\hat{g}_1(X_i)`와 {math}`\hat{g}_0(X_i)`는 부정확하지만, 성향 점수 {math}`\hat{p}(X_i)`는 정확한 경우
[](#decomposed-expectation)에서 성향점수가 정확할 경우, {math}`E[D - \hat{p}(X)] = 0`이 되기 때문에 다음과 같은 항만 남게 됩니다. 여기서부터는 Inverse Propensity Weighting의 성질에 해당하는 부분으로, law of iterated expectations를 이용하면 증명이 가능합니다.

```{math}
E[Y^{DR}(\hat{g},\hat{p})] = E\left[\frac{DY}{\hat{p}(X)}\right] - E\left[\frac{(1-D)Y}{1-\hat{p}(X)}\right]
```

```{math}
= E\left[E\left[\frac{DY}{\hat{p}(X)}\Big| X\right]\right] - E\left[E\left[\frac{(1-D)Y}{1-\hat{p}(X)}\Big| X\right]\right]
```

이 때, {math}`E[DY|X] = \hat{p}(X) * E[Y|D=1, X]`이므로 

```{math}
= E\left[E[Y|D=1, X]\right] - E\left[E[Y|D=0, X]\right] = E\left[E[Y_1,|X]\right] - E\left[E[Y_0|X]\right]
```

다시 한 번 law of iterated expectations를 이용하면, 아래와 같이 정리되어 증명이 완료된다.
```{math}
= E[Y_1] - E[Y_0] = ATE
```