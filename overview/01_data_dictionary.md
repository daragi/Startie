# Startie 데이터 딕셔너리 (Data Dictionary)

## ERD 정렬 기준 (ERD Alignment)
- 본 데이터 딕셔너리는 `04_erd_startie.dbml` 구조를 기준으로 정렬됩니다.
- 스키마 충돌 발생 시 DBML 파일을 구조적 기준(source of truth)으로 사용합니다.  
  (테이블명, PK/FK, null 허용 여부, 관계 방향 우선)

## 1) 분석 범위 및 기간 (Scope and Time Window)
- 데이터 기간: `2025-01` ~ `2025-05` (약 5개월)
- 제공 형식: `12개 CSV 파일`
- 추천 핵심 분석 테이블: `users.csv` + `event_logs.csv`
- 주요 조인 키: `user_id`, `course_id`, `lesson_id`, `experiment_id`, `campaign_id`

## 2) 테이블 구성 개요 (Table Inventory)

| Table | 역할 | 대략 행 수 |
|---|---|---:|
| `users.csv` | 사용자 마스터 프로필 + 라이프사이클 상태 | 19,050 |
| `event_logs.csv` | 제품 행동 이벤트 로그 | 1,125,942 |
| `payment_transactions.csv` | 결제 성공/실패 거래 로그 | 8,177 |
| `plan_history.csv` | 구독 라이프사이클 이력 | 8,585 |
| `courses.csv` | 강의 카탈로그 | 20 |
| `lessons.csv` | 레슨 메타데이터 | 239 |
| `experiments.csv` | A/B 테스트 메타데이터 | 2 |
| `ab_assignment.csv` | 사용자 실험군 배정 정보 | 4,307 |
| `campaigns.csv` | 마케팅 캠페인 정의 + 예산 | 13 |
| `referral_events.csv` | 추천 이벤트 로그 | 1,500 |
| `chat_events.csv` | 고객 상담 기록 | 959 |
| `push_events.csv` | 푸시 발송/오픈 이벤트 | 11,690 |

## 3) 핵심 테이블 (Core Tables)

### 3.1 `users.csv`
**목적**: 거의 모든 분석의 시작점 (유입, 세그먼트, 라이프사이클 분석)

**Primary key**: `user_id`

| Column | Type | 설명 |
|---|---|---|
| `user_id` | str | 사용자 고유 ID (`U-00001` 형식) |
| `signup_date` | date | 가입일 |
| `acquisition_source` | str | 유입 채널 (`organic`, `google_ads`, `meta_ads`, `youtube`, `instagram_influencer`, `referral`, `content_marketing`) |
| `device_type` | str | 주 사용 디바이스 (`web`, `ios`, `android`) |
| `campaign_id` | str | 캠페인 ID (`campaigns` FK, nullable) |
| `signup_method` | str | 가입 방식 (`email`, `google`, `kakao`, `naver`) |
| `gender` | str | 성별 (`male`, `female`, `other`) |
| `age_group` | str | 연령대 (`20-24`, `25-29`, `30-34`, `35-39`, `40+`) |
| `job_category` | str | 직업군 (`student`, `marketing`, `design`, `office_admin` 등) |
| `state` | str | 현재 상태 (`trialing`, `active`, `free_active`, `churned`, `resurrected`) |
| `current_plan` | str | 현재 플랜 (`monthly_basic`, `monthly_pro`, `annual_basic`, `annual_pro`, null) |
| `plan_start_date` | date | 유료 플랜 시작일 (nullable) |
| `is_activated` | bool | Aha Moment 도달 여부 |
| `activation_date` | date | Activation 날짜 (nullable) |
| `churn_date` | date | 이탈 날짜 (nullable) |
| `cancel_reason` | str | 해지 사유 (nullable) |
| `total_sessions` | int | 누적 세션 수 |
| `total_lessons_completed` | int | 누적 완료 레슨 수 |
| `last_active_date` | date | 마지막 활동일 |

**Notes**
- `is_activated`는 Activation/Aha 분석 핵심 변수
- `state`, `current_plan`, `churn_date`는 함께 해석 필요

---

### 3.2 `event_logs.csv`
**목적**: 퍼널, Activation, 리텐션, 행동 분석용 전체 이벤트 스트림

**Grain**: 이벤트 1건당 1행

| Column | Type | 설명 |
|---|---|---|
| `user_id` | str | 사용자 ID (`users` FK) |
| `session_id` | str | 세션 ID |
| `event_name` | str | 이벤트명 |
| `event_timestamp` | datetime | 이벤트 발생 시각 |
| `device_type` | str | 이벤트 발생 디바이스 |
| `location` | str | 위치 정보 |
| `event_sequence` | int | 세션 내 이벤트 순서 |
| `page_name` | str | 페이지명 (nullable) |
| `course_id` | str | 강의 ID (`courses` FK, nullable) |
| `lesson_id` | str | 레슨 ID (`lessons` FK, nullable) |
| `event_properties` | str(JSON) | 추가 속성 JSON |

**event_properties 예시**
- 플랜 선택: `{"plan_name": "monthly_pro"}`
- 퀴즈: `{"quiz_id": "Q-012", "is_correct": true, "score": 85}`
- 검색: `{"search_keyword": "python", "result_count": 8}`
- 온보딩: `{"step": 2}`

**Notes**
- JSON 파싱 시 richer feature 생성 가능
- 대용량이므로 필요한 컬럼만 선별 로딩 권장

---

### 3.3 `payment_transactions.csv`
**목적**: 결제 결과 및 LTV / 매출 분석

**Primary key**: `transaction_id`

| Column | Type | 설명 |
|---|---|---|
| `transaction_id` | str | 거래 ID |
| `user_id` | str | 사용자 ID |
| `transaction_date` | datetime | 거래 시각 |
| `plan_name` | str | 구매 플랜 |
| `amount` | int | 결제 금액 (KRW) |
| `currency` | str | 통화 (`KRW`) |
| `payment_method` | str | 결제 수단 (`card`) |
| `status` | str | `completed` / `failed` |
| `failure_reason` | str | 실패 사유 (nullable) |
| `retry_count` | int | 재시도 횟수 |

## 4) 보조 테이블 (Supporting Tables)

### 4.1 `plan_history.csv`
구독 상태 변화 이력

| Column | Type | 설명 |
|---|---|---|
| `user_id` | str | 사용자 ID |
| `plan_name` | str | 플랜명 |
| `action` | str | `subscribe`, `renew`, `upgrade`, `cancel` |
| `action_date` | date | 실행 날짜 |
| `previous_plan` | str | 이전 플랜 |
| `cancel_reason` | str | 해지 사유 |

---

### 4.2 `ab_assignment.csv`
실험군 배정 정보

| Column | Type | 설명 |
|---|---|---|
| `user_id` | str | 사용자 ID |
| `experiment_id` | str | 실험 ID |
| `variant` | str | `control` / `treatment` |
| `assigned_date` | date | 배정일 |

---

### 4.3 `experiments.csv`
실험 메타데이터

| Column | Type | 설명 |
|---|---|---|
| `experiment_id` | str | 실험 ID |
| `experiment_name` | str | 실험명 |
| `hypothesis` | str | 가설 |
| `primary_metric` | str | 주요 지표 |
| `variants` | str(JSON) | 실험군 정의 |
| `traffic_pct` | float | 트래픽 비율 |
| `start_date` | date | 시작일 |
| `end_date` | date | 종료일 |
| `target_segment` | str | 대상 세그먼트 |
| `status` | str | `running` / `completed` |

Known experiments:
- 온보딩 개편 실험
- 가격 페이지 실험

## 5) 이벤트 분류 (`event_logs.event_name`)

### Lifecycle
- `signup_completed`
- `onboarding_started`
- `onboarding_step_completed`
- `onboarding_completed`

### Learning
- `page_viewed`
- `course_searched`
- `course_detail_viewed`
- `course_wishlisted`
- `lesson_started`
- `lesson_completed`
- `quiz_submitted`
- `course_completed`
- `certificate_downloaded`

### Monetization
- `pricing_page_viewed`
- `plan_selected`
- `checkout_started`

### Engagement
- `chat_started`
- `review_submitted`
- `referral_sent`

### Session
- `session_started`
- `session_ended`

## 6) 실무 Join Map

- `users.user_id = event_logs.user_id`
- `users.user_id = payment_transactions.user_id`
- `users.user_id = plan_history.user_id`
- `users.user_id = ab_assignment.user_id`
- `experiments.experiment_id = ab_assignment.experiment_id`

## 7) 데이터 품질 주의사항

- nullable 컬럼 주의:
  `campaign_id`, `plan_start_date`, `activation_date`, `churn_date`, `opened_at`
- retention/cohort 분석 전 timezone 확인
- user-level device와 event-level device 구분
- one-to-many join 시 duplication bias 주의
- event_logs는 필요한 컬럼만 먼저 select 권장

## 8) 추천 Base Mart

- `user_daily_activity`: 사용자 × 날짜 행동 집계
- `user_funnel`: signup → onboarding → lesson → checkout
- `user_monetization`: payment + plan history 사용자 단위 집계
- `experiment_user_outcomes`: 실험군별 outcome window
- `channel_quality`: 유입 채널 × retention × revenue