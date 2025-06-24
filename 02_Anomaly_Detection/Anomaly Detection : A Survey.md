# Anomaly Detection : A Survey

> **Bibliography**: Chandola, V. 2009. _Anomaly Detection: A Survey_.
> 
> **Authors**:  [Varun Chandola]
> 

---

## 1. Introduction

- 이상 탐지(Anomaly Detection)는 정상 패턴에서 벗어난 데이터(이상치, outlier)를 식별하는 문제로, 다양한 응용 분야(사기 탐지, 침입 탐지, 결함 탐지 등)에서 중요하다.
- 이상 탐지는 다양한 데이터 타입, 도메인, 문제 정의에 따라 다양한 접근법이 존재한다.
- 본 논문은 이상 탐지의 주요 개념, 방법론, 응용, 도전 과제 등을 포괄적으로 정리한다.

---

## 2. Anomaly Detection: Problem Definition

### 2.1 What are Anomalies?

- **이상치(Anomalies):** 정상 패턴에서 벗어난 데이터. Outlier, exception, novelty 등으로 불리기도 한다.
- 이상치를 **정상 영역**으로 구분하고 정상과 다른 관측치를 이상으로 선언하는 접근 방식이 기본이다.
- 그러나 **정상 영역을 정의하는 것**은 매우 어렵고, 정상과 비정상 행동의 경계가 명확하지 않아 이 경계 근처의 이상 관측치가 실제로는 정상일 수 있다.
- 악의적인 행동으로 인한 이상 시, **악의적인 주체가 정상처럼 보이도록 행동**을 조정하기 때문에 정상 행동의 정의가 더욱 복잡해진다.
- 많은 분야에서 정상 행동은 **지속적으로 진화**하므로 현재의 정상 개념이 미래에 충분히 대표적이지 않을 수 있다. 
- 이상 개념은 다양한 **응용 분야마다 다르게 정의**되므로, 한 분야에서 개발된 기법을 다른 분야에 적용하기 어렵다.
- 이 외에도 모델 학습/검증을 위한 **라벨링된 데이터의 가용성**과 데이터 내 노이즈로 인해 실제 이상과 구별하기 어려운 경우가 많아 이상 탐지문제 해결이 복잡해진다.

### 2.2 Types of Anomalies

- **Point Anomalies:** 개별 데이터가 전체 집합에서 이질적일 때
![[Pasted image 20250624224945.png]]
- **Contextual Anomalies:** Conditional Anomalies, 특정 context에서만 이질적인 경우 
![[Pasted image 20250624225010.png]]
- **Collective Anomalies:** 데이터 집합이 비정상적 패턴을 보일 때
![[Pasted image 20250624225032.png]]
## 2.3 Problem Formulation

**1. Data Labels**

- Supervised anomaly detection
- Semi-supervised anomaly detection
- Unsupervised anomaly detection : Train data가 필요 없으며, normal이 abnormal보다 훨씬 많다는 암묵적인 가정하게 이루어지는 학습

**2. Output** 

* Score : 특정 threshold를 선택하여 이상 탐지를 할 수 있다
* Labels : category label을 할당하는 방법 (normal or anomalous)

---

## 3. Taxonomy of Anomaly Detection Techniques

### 3.1 Classification based Techniques

**분류(Classification) 기반 이상 탐지 기법**은 정상(normal)과 이상(anomaly) 데이터를 구분하는 분류 모델을 학습하여 이상 탐지를 수행하는 접근법이다. 이 방식은 레이블이 부여된 데이터(정상/이상)가 존재하는 경우에 가장 효과적이다.

**1. Supervised Classification (지도 학습 기반 분류)**

- **정의:** 정상과 이상 데이터 모두에 대해 레이블이 존재할 때 적용.
- **방법:** 이진 분류(binary classification) 문제로 간주하여, 정상/이상 데이터를 각각의 클래스로 분류하는 모델을 학습.
- **주요 기법:** Decision Tree, Support Vector Machine(SVM), Neural Networks 등 전통적인 분류 알고리즘 사용.
- **장점:** 충분한 레이블 데이터가 있다면 높은 정확도 가능.
- **단점:** 실제로는 이상 데이터의 수가 매우 적어 데이터 불균형 문제가 심각하며, 새로운 유형의 이상(anomaly)에 대한 탐지가 어렵다.
    
 **2. Semi-supervised Classification (준지도 학습 기반 분류)**

- **정의:** 정상 데이터에 대한 레이블만 존재하고, 이상 데이터는 레이블이 없는 경우에 사용.
- **방법:** 정상 데이터만을 이용해 정상 패턴을 학습한 후, 정상 패턴에서 벗어나는 데이터를 이상으로 간주.
- **주요 기법:** One-class SVM, Autoencoder 등.
- **장점:** 실제 상황에서 이상 데이터의 레이블을 얻기 어려운 문제를 해결.
- **단점:** 정상 패턴이 충분히 대표적이지 않거나, 정상 패턴이 변화하는 경우 탐지 성능 저하.
    
**3. 분류 기반 접근법의 특징**

- **데이터 불균형 문제:** 이상 데이터가 정상 데이터에 비해 매우 적어, 분류기의 성능이 저하될 수 있음.
- **새로운 이상 탐지의 한계:** 학습 데이터에 없는 새로운 유형의 이상은 탐지하기 어려움.
- **레이블 의존성:** 충분한 레이블 데이터 확보가 어려운 경우가 많음.

### 3.2 Nearest Neighbor based Techniques

**최근접 이웃(Nearest Neighbor, NN) 기반 이상 탐지 기법**은 데이터 포인트가 데이터 집합 내의 다른 포인트들과 얼마나 가까운지(혹은 먼지)를 측정하여, 정상 패턴에서 벗어난 데이터를 이상치로 간주하는 방법이다.

**[주요 개념]**

- **기본 가정:** 정상 데이터는 서로 가까운 이웃을 많이 가지며, 이상 데이터는 정상 데이터와 멀리 떨어져 있어 가까운 이웃이 적거나, 이웃까지의 거리가 멀다.
- **핵심 아이디어:** 각 데이터 포인트에 대해 k-최근접 이웃(k-NN)까지의 거리 또는 이웃의 밀도를 계산하여, 이 값이 크거나 밀도가 낮으면 이상으로 판단.
    
**[대표적인 방법]**

1. **k-Nearest Neighbor Distance**
    
    - 각 데이터 포인트에서 k번째로 가까운 이웃까지의 거리를 이상 점수로 사용.
    - 거리가 크면(이웃이 멀리 있으면) 이상치로 간주.
        
2. **Relative Density**
    
    - 각 포인트의 이웃 밀도(density)를 계산하고, 이 값이 주변보다 현저히 낮으면 이상치로 판단.
    - 예시: Local Outlier Factor (LOF) – 각 포인트의 밀도를 주변 이웃과 비교하여 이상 점수를 산출.
        
3. **Distance to Centroid**
    
    - 전체 데이터의 중심(centroid)에서 각 포인트까지의 거리를 측정하여, 거리가 먼 포인트를 이상치로 간주.

**[장점]**
- **비지도 학습:** 레이블이 필요 없고, 데이터 분포에 대한 사전 가정이 필요 없음.
- **비선형 구조 탐지:** 복잡한 데이터 구조에서도 이상 탐지가 가능.

**[단점]**
- **고차원 데이터:** 차원의 저주(curse of dimensionality)로 인해 거리 계산의 효과가 떨어질 수 있음.
- **계산 비용:** 모든 포인트 쌍의 거리를 계산해야 하므로 데이터가 많을수록 계산량이 급증.
- **파라미터 민감성:** k 값 등 파라미터 설정에 따라 결과가 달라질 수 있음.


### 3.3 Clustering based Techniques

- 군집 내 데이터는 정상, 군집 밖 데이터는 이상


### 3.4 Statistical Methods

- 데이터 분포(가우시안 등) 모델링, 확률 낮은 데이터는 이상


### 3.5 Information Theoretic Approaches

- 엔트로피, 압축률 등 정보량의 변화로 이상 탐지


## 3.6 Spectral Techniques

- 데이터 행렬의 고유값/고유벡터 분석, 차원축소 기반 이상 탐지


---

## 4. Application Domains

- **Intrusion Detection:** 네트워크, 시스템 로그 등에서 이상 행동 탐지

- **Fraud Detection:** 신용카드, 보험, 금융 거래 등

- **Medical and Public Health Anomaly Detection:** 환자 기록, 질병 발생 등

- **Industrial Damage Detection:** 센서 데이터 기반 결함 탐지 등

- **기타:** Image, Video, Text, Sensor 등 다양한 도메인

## 5. Challenges in Anomaly Detection

- **데이터의 고차원성 및 복잡성**

- **정상/이상 경계의 모호함**

- **레이블 부족**

- **정상 패턴의 변화(Non-stationarity)**
   
- **이상 데이터의 다양성 및 불균형**


---

## 6. Related Problems

- **노이즈(noise)와 이상치(outlier)의 구분**

- **변칙 감지와 노벨티 감지(novelty detection)의 차이**

- **이상치 수정(outlier correction) 등**


---

## 7. Conclusions

- 이상 탐지는 다양한 분야에서 필수적이며, 각 접근법의 장단점과 적용 조건을 이해하는 것이 중요하다.
- 향후 연구 방향: 복합적 이상 탐지, 고차원/비정형 데이터, 설명 가능한 이상 탐지 등

