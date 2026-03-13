# Startie 분석 가이드 (Analysis Guide)

## 1) 목적 (Objective)
이 가이드는 Startie의 프로덕트 분석을 어떻게 수행할지 정의하며, CEO의 5가지 핵심 질문에 대해 **의사결정 가능한 결과물(decision-ready outputs)** 을 만드는 것을 목표로 합니다.

이 가이드를 통해 도출해야 할 산출물:
- 재현 가능한 지표 정의 로직
- 세그먼트 기반 인사이트 (채널, 디바이스, 코호트, 플랜)
- 기대 효과를 포함한 실행 권고안

## 2) 분석 운영 원칙 (Analysis Operating Principles)
- 차트를 그리기 전에 항상 **지표의 분자/분모**를 먼저 정의합니다.
- 최소 2개 이상의 세그먼트 축을 함께 비교합니다.  
  (예: 채널 + 디바이스)
- 비율 지표(rate, ARPU)와 함께 **모수(N users)** 도 같이 봅니다.
- 이벤트 기반 추론과 결제/구독 진실 테이블(truth table)은 분리해서 해석합니다.
- 단일 시점 스냅샷보다 **코호트 기반 비교**를 우선합니다.

## 3) 권장 분석 워크플로우 (Recommended Workflow)
1. 데이터 준비 및 QA
2. 핵심 퍼널 및 Activation 분석
3. 리텐션 / 이탈 진단
4. 수익화 및 채널 품질 분석
5. 실험(A/B 테스트) 평가
6. 종합: 다음 분기 단일 우선순위 + 실행 계획 제안

## 4) 데이터 준비 체크리스트 (Data Readiness Checklist)

### 4.1 로드 순서 (Load order)
1. `users.csv`
2. `event_logs.csv` (필요 컬럼만 선택 로드)
3. `payment_transactions.csv` + `plan_history.csv`
4. 선택 로드: `ab_assignment.csv`, `experiments.csv`, `campaigns.csv` 등

### 4.2 핵심 QA 체크 항목 (Core QA checks)
- 유일성 확인: `users.user_id`, `payment_transactions.transaction_id`
- Null 점검: `campaign_id`, `activation_date`, `churn_date`, `opened_at`
- 날짜 범위 검증: 모든 테이블이 `2025-01` ~ `2025-05` 범위 내인지 확인
- Enum 검증: channel / device / state / action / status 값 검증
- Join sanity: one-to-many join 이후 행 수 증가(row inflation) 여부 확인

## 5) 표준 지표 정의 (Canonical Metric Definitions)

### 5.1 유입 및 활동성 (Acquisition and activity)
- `Signup Users`: `users.user_id`의 고유 개수
- `MAU`: 월별로 최소 1회 이상 이벤트를 발생시킨 고유 사용자 수
- `WAU`: 주별로 최소 1회 이상 이벤트를 발생시킨 고유 사용자 수

### 5.2 Activation (Aha)
- `Activation Rate`: 활성화 유저 수 / 가입 유저 수
- `Activation TTV (time-to-value)`: `activation_date - signup_date`
- 주요 세그먼트 기준: 유입 채널, 주 사용 디바이스, 가입 코호트 주차

### 5.3 퍼널 (예시 Funnel)
- Step 1: `signup_completed`
- Step 2: `onboarding_completed`
- Step 3: `lesson_started`
- Step 4: `lesson_completed`
- Step 5: `pricing_page_viewed`
- Step 6: `plan_selected`
- Step 7: `checkout_started`
- 최종 결과 truth: `payment_transactions.status='completed'` 기준 결제 성공

### 5.4 리텐션 및 이탈 (Retention and churn)
- `D7 Retention`: 가입 코호트 중 7일차에 활동한 사용자 / 가입 코호트 사용자
- `D30 Retention`: 가입 코호트 중 30일차에 활동한 사용자 / 가입 코호트 사용자
- `Churn Rate (period)`: 해당 기간 이탈 사용자 / 해당 기간 활성 또는 체험 사용자
- `Churn Reason Mix`: `cancel_reason` 분포

### 5.5 수익화 (Monetization)
- `Paid Conversion Rate`: 1회 이상 결제 성공한 사용자 / 체험 시작 사용자  
  (또는 가입자 기준도 가능하나 반드시 명시)
- `ARPPU`: 총 매출 / 유료 결제 사용자 수
- `ARPU`: 총 매출 / 활성 사용자 수  
  (또는 전체 가입자 수 기준 가능하나 반드시 명시)
- `LTV (observed window)`: 관측 기간 내 사용자별 누적 결제 금액

### 5.6 채널 품질 (Channel quality)
- `Volume`: `acquisition_source`별 가입자 수
- `Quality`: Activation, Paid Conversion, D30 Retention, Observed LTV
- `Efficiency proxy`: 비용 데이터가 있는 경우 캠페인/채널별 LTV/CAC

## 6) CEO 질문별 분석 플레이북 (CEO Question Playbooks)

## Q1. 사용자는 실제로 가치를 경험하고 있는가?
**Goal**: Activation과 유료 전환에 가장 강하게 연결된 행동을 식별

### Steps
1. 활성화 유저 vs 비활성화 유저의 가입 후 7일 행동 수 비교
2. Aha 후보 임계값 테스트  
   (예: 완료 레슨 수 >= N, 온보딩 완료, 검색 + 레슨 시퀀스)
3. 후보별 전환율 상승 효과를 디바이스별로 평가

### Outputs
- 운영 가능한 Aha 정의 제안
- 디바이스/채널별 Activation 퍼널
- 유료 전환의 주요 선행 행동 지표

---

## Q2. 어떤 채널이 가장 가치 높은 사용자를 데려오는가?
**Goal**: 유입량이 큰 채널과 품질이 좋은 채널을 구분

### Steps
1. 채널별 가입자 수, Activation, D30 Retention, Paid Conversion, Observed LTV 테이블 생성
2. 가능하면 캠페인 예산을 붙여 CAC 추정
3. 표본 수 신뢰도와 함께 LTV/CAC 기준 채널 순위 산출

### Outputs
- 채널 포트폴리오 매트릭스 (Volume vs Quality)
- 예산 재배분 권고안

---

## Q3. 사용자는 왜 이탈하는가?
**Goal**: 이탈 직전 행동 패턴과 예방 가능한 원인을 탐지

### Steps
1. 이탈 직전 분석 구간 정의  
   (예: `churn_date` 이전 14일)
2. 이탈 유저 vs 비이탈 유저의 행동 시그널 비교
3. 채널/디바이스/플랜별 `cancel_reason` 분포 분석

### Outputs
- 고위험 행동 마커
- 예방 가능한 이탈 사유 기회 영역
- 리텐션 개입 우선순위 후보

---

## Q4. A/B 테스트 결과를 실제로 롤아웃해야 하는가?
**Goal**: 실험 결과를 명확한 제품 의사결정으로 전환

### Steps
1. `ab_assignment` + 배정일 기준 실험 코호트 생성
2. 실험 기간에 맞는 primary metric window 일관되게 정의
3. control vs treatment 차이, 신뢰구간, 실질 효과 크기 계산
4. 핵심 세그먼트별로 이질적 효과(heterogeneous effect) 확인

### 의사결정 템플릿 (Decision rule template)
- 가드레일(ARPU/Retention/Churn)까지 포함해  
  **통계적으로 신뢰 가능하고 비즈니스적으로 긍정적이면 롤아웃**
- 효과가 불안정하거나 핵심 세그먼트에서 음수면 보류/재실험

### Outputs
- 실험별 `rollout / iterate / stop` 권고안

---

## Q5. 다음 분기 단 하나의 우선순위는 무엇인가?
**Goal**: 여러 분석 결과를 종합해 가장 영향력 큰 액션 1개 선정

### Steps
1. 이니셔티브를 `impact × confidence × implementation effort` 기준으로 점수화
2. 1개의 핵심 베팅과 2~3개의 보조 실행안 선정
3. KPI 목표와 리뷰 주기 정의

### Outputs
- 1페이지 분기 집중 과제 제안서
- KPI 목표 테이블 (baseline → target)

## 7) 추천 분석 산출물 (Suggested Analysis Artifacts)
- `cohort_retention.csv`: 가입 코호트 × 리텐션 일자
- `activation_candidates.csv`: 후보 정의 × 전환 상승 효과
- `channel_scorecard.csv`: 채널 × 유입량/품질/효율
- `churn_risk_features.csv`: 사용자 단위 pre-churn feature set
- `ab_test_results.csv`: 실험 × 지표 × 실험군 × 효과 크기

## 8) 최소 SQL / Pandas 모델링 스펙 (Minimal SQL/Pandas Modeling Specs)
- 먼저 **사용자 단위 마트(`one row per user`)** 를 구축  
  포함 항목: acquisition, state, activation, churn, monetization
- 이후 이벤트 기반 집계를 기간별로 추가
  - 첫 1일
  - 첫 7일
  - 첫 30일
  - 이탈 전 14일
- 결제 truth는 최종 결합 단계 전까지 event funnel과 분리 유지

## 9) CEO / 이사회 발표 템플릿 (Presentation Template)
1. 핵심 질문과 그 중요성
2. 지표 정의와 범위
3. 주요 결과 (세그먼트 분해 포함)
4. 비즈니스 해석
5. 권장 액션과 기대 효과
6. 리스크 / 가정 / 추가 검증 단계

## 10) 자주 발생하는 실수 (Common Pitfalls)
- 최종 결제/이탈 결론을 `event_logs`만으로 내리는 것
- 코호트 연령이 다른 세그먼트를 직접 비교하는 것
- 채널/실험 비교에서 표본 수를 무시하는 것
- user-level 분모와 event-level 분모를 혼합하는 것
- 불확실성 없이 point estimate만 보고하는 것

## 11) 5일 실행 계획 (Suggested 5-Day Execution Plan)
- Day 1: 데이터 로드, QA, 베이스 마트 구축
- Day 2: Activation + 퍼널 + 초기 전환 분석
- Day 3: 리텐션/이탈 + 채널 품질 분석
- Day 4: A/B 테스트 평가 + 종합 인사이트 정리
- Day 5: 스토리라인 구성, 액션 플랜 작성, 최종 리뷰