# CEO 질문별 플레이북 (이사회 보고용)

## 문서 목적 (Document Purpose)
이 문서는 CEO의 5가지 핵심 질문을  
**분석 → 권고안 도출 → 의사결정** 으로 연결하는 실행 템플릿입니다.

각 질문은 아래 구조로 정리합니다.
```
1. 비즈니스 질문
2. 왜 중요한가
3. 지표 정의
4. 분석 방법
5. 결과 요약
6. 권고안
7. 기대 효과
8. 리스크 / 가정
```
---

## Q1) “사용자는 우리 제품에서 실제로 가치를 경험하고 있는가?”

### 왜 중요한가 (Why this matters)
사용자가 초기에 가치를 경험하지 못하면,  
무료 체험 → 유료 전환율과 리텐션은 계속 약할 수밖에 없습니다.

### 핵심 지표 (Key metrics)
- Activation Rate (`is_activated=True` / signups)
- Time-to-Activation (`activation_date - signup_date`)
- 초기 참여도 (가입 후 7일 내 레슨 시작/완료 수)
- 무료 체험 → 유료 전환율

### 필요한 데이터 (Required data)
- `users.csv`
- `event_logs.csv`
- `payment_transactions.csv`

### 분석 접근 방식 (Analysis approach)
1. 활성화 유저와 비활성화 유저의 가입 후 7일 행동을 비교합니다.
2. Aha 후보를 테스트합니다.  
   (예: 온보딩 완료, 특정 수 이상의 레슨 완료 등)
3. 후보별 전환 상승 효과를 디바이스/채널 기준으로 추정합니다.

### 결과 출력 형식 (Output format)
- 퍼널 차트: signup → onboarding → lesson → paid
- 표: Aha 후보 vs 전환 상승 효과
- 세그먼트 분해: `device_type`, `acquisition_source`

### 의사결정 기준 (Decision criteria)
- 전환과의 연관성이 가장 강하고 안정적인 **운영 가능한 Aha 정의 1개**를 선택합니다.

### 권고안 템플릿 (Recommendation template)
- “Activation은 가입 후 ______일 이내 ________ 달성으로 정의한다.”
- “이 행동을 ___%p 높이는 기능 개선을 우선 추진한다.”

---

## Q2) “어떤 유입 채널이 가장 가치 높은 사용자를 데려오는가?”

### 왜 중요한가 (Why this matters)
유입량이 큰 채널이 항상 좋은 채널은 아닙니다.  
예산은 단순 트래픽이 아니라 **가치(value)** 를 따라가야 합니다.

### 핵심 지표 (Key metrics)
- 채널별 가입자 수
- Activation Rate
- D30 Retention
- Paid Conversion
- Observed LTV
- LTV/CAC (캠페인 비용이 있는 경우)

### 필요한 데이터 (Required data)
- `users.csv`
- `payment_transactions.csv`
- `campaigns.csv`
- `event_logs.csv` (리텐션/활동성 분석용)

### 분석 접근 방식 (Analysis approach)
1. 채널 스코어카드를 작성합니다.  
   (유입량 + 품질 + 효율)
2. 유입량 기준 순위와 LTV/CAC 기준 순위를 비교합니다.
3. 표본 수와 코호트 연령 일관성을 검증합니다.

### 결과 출력 형식 (Output format)
- 매트릭스: Volume (x) vs Quality (y)
- 순위표: channel scorecard
- 예산 이동 시나리오 (현재 vs 제안)

### 의사결정 기준 (Decision criteria)
- 품질과 효율이 모두 기준을 넘는 채널은 예산을 확대합니다.
- 유입량은 크지만 품질이 지속적으로 낮은 채널은 예산을 축소합니다.

### 권고안 템플릿 (Recommendation template)
- “예산의 ___%를 ______ 채널에서 ______ 채널로 재배분한다.  
  기대 효과: 유료 전환율 +___%, LTV +___원”

---

## Q3) “사용자는 왜 이탈하며, 무엇을 예방할 수 있는가?”

### 왜 중요한가 (Why this matters)
이탈을 줄이는 것은 보통 신규 유입을 늘리는 것보다  
더 효율적으로 매출을 개선할 수 있습니다.

### 핵심 지표 (Key metrics)
- 기간별 Churn Rate
- 이탈 사유 분포 (`cancel_reason`)
- 이탈 직전 행동 신호 (최근 14일)
- 디바이스/플랜/채널별 이탈 격차

### 필요한 데이터 (Required data)
- `users.csv`
- `plan_history.csv`
- `event_logs.csv`
- `chat_events.csv` (선택, 이슈 맥락 파악용)

### 분석 접근 방식 (Analysis approach)
1. 이탈 코호트와 이탈 전 관측 구간을 정의합니다.
2. 이탈 유저 vs 비이탈 유저의 행동 패턴을 비교합니다.
3. 예방 가능한 주요 이탈 사유와 취약 세그먼트를 식별합니다.

### 결과 출력 형식 (Output format)
- 이탈 사유 Pareto 차트
- 세그먼트 표: device/channel/plan별 churn rate
- 이탈 전 행동 신호 목록 (상위 예측 변수)

### 의사결정 기준 (Decision criteria)
- **빈도가 높고 예방 가능성이 큰 이탈 사유**를 우선 개입 대상으로 선정합니다.

### 권고안 템플릿 (Recommendation template)
- “세그먼트 ______에 대해, 신호 ______ 발생 시 리텐션 개입을 실행한다.  
  기대 이탈 감소 효과: ___%p”

---

## Q4) “최근 A/B 테스트 변경사항을 실제로 롤아웃해야 하는가?”

### 왜 중요한가 (Why this matters)
약하거나 오해를 부르는 실험 결과를 롤아웃하면  
장기적으로 비즈니스에 손해를 줄 수 있습니다.

### 핵심 지표 (Key metrics)
- 주요 지표 상승폭 (실험 정의 기준)
- 신뢰구간 / 통계적 유의성
- 가드레일: ARPU, 리텐션, 이탈률
- 세그먼트별 이질적 효과

### 필요한 데이터 (Required data)
- `ab_assignment.csv`
- `experiments.csv`
- `event_logs.csv`
- `payment_transactions.csv`
- `users.csv`

### 분석 접근 방식 (Analysis approach)
1. 배정일 기준으로 깨끗한 실험 코호트를 구성합니다.
2. treatment vs control을 동일한 관측 구간으로 비교합니다.
3. 실질적 효과 크기와 가드레일 영향까지 함께 봅니다.
4. 핵심 세그먼트별 효과 안정성을 검증합니다.

### 결과 출력 형식 (Output format)
- 실험 결과 표 (control / treatment / delta / CI)
- 세그먼트 효과 heatmap
- 롤아웃 의사결정 노트

### 의사결정 기준 (Decision criteria)
- `Rollout`: 통계적으로 신뢰 가능하고, 비즈니스적으로 긍정적이며, 핵심 가드레일 훼손이 없음
- `Iterate`: 결과가 혼합적이거나 세그먼트 의존적임
- `Stop`: 상승 효과가 없거나 비즈니스 영향이 음수임

### 권고안 템플릿 (Recommendation template)
- “결정: Rollout / Iterate / Stop  
  근거: ________  
  다음 테스트: ________”

---

## Q5) “다음 분기의 단 하나의 최우선 과제는 무엇인가?”

### 왜 중요한가 (Why this matters)
하나의 집중된 베팅은 실행력을 높이고  
성과 측정을 더 명확하게 만듭니다.

### 핵심 지표 (Key metrics)
- 예상 임팩트 (매출 / 리텐션 / 전환)
- 신뢰도 (근거 강도)
- 구현 난이도
- 성과 측정까지 걸리는 시간

### 필요한 데이터 (Required data)
- Q1~Q4 분석 결과 종합

### 분석 접근 방식 (Analysis approach)
1. 앞선 분석에서 나온 실행 후보를 정리합니다.
2. 각 과제를 `Impact x Confidence x Effort` 기준으로 점수화합니다.
3. 최우선 과제 1개를 선정하고 마일스톤을 정의합니다.

### 결과 출력 형식 (Output format)
- 우선순위 점수표
- 90일 KPI 로드맵 (baseline → target)
- 담당자 / 리뷰 주기 운영안

### 의사결정 기준 (Decision criteria)
- **가장 레버리지가 크고 실행 가능성 높은 1개 과제**를 선택합니다.

### 권고안 템플릿 (Recommendation template)
- “다음 분기 최우선 과제는 ________ 이다.  
  분기 말 목표: ________  
  선행 지표: ________”

---

## 최종 이사회 슬라이드 구성안 (Suggested)
1. Executive summary (핵심 결정 3가지)
2. Q1 가치 경험 분석
3. Q2 채널 품질 분석
4. Q3 이탈 진단
5. Q4 실험 의사결정
6. Q5 다음 분기 우선순위
7. KPI 목표표 + 액션 플랜
8. 리스크 및 모니터링 계획

## KPI 목표표 템플릿 (KPI Target Table Template)

| KPI | Current | Target (Next Quarter) | Owner | Cadence |
|---|---:|---:|---|---|
| Activation Rate |  |  |  | Weekly |
| Trial->Paid Conversion |  |  |  | Weekly |
| D30 Retention |  |  |  | Monthly |
| Churn Rate |  |  |  | Weekly |
| ARPU |  |  |  | Monthly |
| LTV/CAC |  |  |  | Monthly |

## Notes
- 모든 지표 정의는 `03_analysis_guide.md`와 일관되게 유지합니다.
- 수익화/이탈의 기준 데이터는 `payment_transactions`, `plan_history`를 source of truth로 사용합니다.
- 방향성 인사이트를 제시할 때도 항상 **세그먼트와 표본 수 맥락**을 함께 제시합니다.