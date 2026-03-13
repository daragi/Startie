# Startie Product Overview

## 1) 제품 요약 (Product Summary)
> **Startie**는 20~30대 커리어 전환 준비자를 위한 온라인 실무 교육 플랫폼입니다.

- 핵심 가치 제안: 커리어 전환 준비자가 실무 역량을 빠르게 갖출 수 있도록 지원
- 주요 콘텐츠 카테고리: `programming`, `data`, `business`, `design`, `ai`
- 제공 방식: 구독형 온라인 학습 서비스
- 핵심 사용자 프로필: 비전공자 기반 데이터 / 개발 / 기획 직무 전환 준비자

## 2) 비즈니스 모델 (Business Model)
> Startie는 **7일 무료 체험 → 유료 구독 전환** 구조로 운영됩니다.

### 가격 정책 (Pricing)
- `Basic`: 월 9,900원 / 연 99,000원
- `Pro`: 월 19,900원 / 연 199,000원
- 무료 체험 기간 동안 Pro 기능 제공

## 3) 현재 비즈니스 상황 (Current Business Context)
> Startie는 시드 투자 이후 빠른 성장 압박이 있는 단계입니다.

- 투자 현황: 시드 투자 10억 원 유치 완료
- 서비스 운영 기간: 런칭 후 약 10개월
- 누적 가입자 수: 약 20,000명
- MAU: 약 5,000명
- 전략적 과제: 향후 약 6개월 내 투자자에게 의미 있는 지표 개선 필요

## 4) 해결해야 할 문제 (Why Analysis Now)
>경영진은 약 4주 뒤 이사회 보고를 위해 데이터 기반 의사결정 근거가 필요합니다. 현재 핵심 과제는 단순 성장 규모가 아니라 **성장의 질**입니다.

- 사용자가 빠르게 가치를 경험하고 있는가?
- 어떤 사용자/채널이 지속 매출을 만드는가?
- 왜 사용자가 이탈하는가?
- 어떤 실험을 확대 적용해야 하는가?
- 다음 분기 가장 우선해야 할 핵심 레버는 무엇인가?

## 5) CEO 핵심 질문 프레임워크 (Analysis Themes)
> 본 프로젝트는 5개의 CEO 수준 질문을 중심으로 진행됩니다.

### 1. 사용자 가치 경험 (User Value Realization)
- Activation / Aha Moment 정의
- 어떤 행동이 유료 전환과 연결되는가?
- 디바이스별 초기 가치 경험 차이 분석

### 2. 채널 품질 분석 (Channel Quality)
- 유입 채널별 리텐션 및 수익성 비교
- 채널별 LTV vs CAC 분석
- 유입량 대비 품질 비교

### 3. 이탈 이해 및 방지 (Churn Understanding and Prevention)
- 이탈 직전 행동 패턴 분석
- 디바이스별 이탈 차이
- 주요 해지 사유 파악

### 4. A/B 테스트 의사결정 (A/B Test Decisioning)
- 온보딩 개선 실험의 통계적 검증
- 가격 페이지 실험: 전환율 vs ARPU 트레이드오프
- 세그먼트별 실험 효과 차이 분석

### 5. 다음 분기 우선순위 (Next-quarter Priority)
- 가장 영향력 큰 개선 과제 도출
- 제품 개선 효과가 코호트에 반영되는지 검토
- 기대 효과를 포함한 구체적 실행안 제안

## 6) 데이터 범위 개요 (Data Coverage Snapshot)
> 분석 데이터는 `2025-01` ~ `2025-05` 약 5개월 운영 데이터를 포함하며 총 12개 CSV 테이블로 구성됩니다.

- 전체 사용자 수: 약 19,000명 (`기존 5천명 + 신규 1만4천명`)
- 이벤트 로그: 약 1,126,000건
- 유입 채널: 7개  
  (`organic`, `google_ads`, `meta_ads`, `youtube`, `instagram_influencer`, `referral`, `content_marketing`)
- 디바이스: 3개  
  (`web` 45%, `ios` 30%, `android` 25%)
- 실험: 2개  
  (온보딩 개편 / 가격 페이지 실험)

### 핵심 분석 테이블 (Core analysis tables)
- `users.csv`: 사용자 프로필, 상태, 플랜, 활성화/이탈 정보
- `event_logs.csv`: 퍼널 / 리텐션 / 행동 분석용 이벤트 로그

### 보조 테이블 (Supporting tables)
- 수익화: `payment_transactions.csv`, `plan_history.csv`
- 실험: `experiments.csv`, `ab_assignment.csv`
- 콘텐츠: `courses.csv`, `lessons.csv`
- 마케팅/CRM: `campaigns.csv`, `referral_events.csv`, `push_events.csv`, `chat_events.csv`

## 7) 본 프로젝트에서 좋은 결과의 기준 (What “Good” Looks Like)
> 본 프로젝트의 성공 기준은 다음과 같습니다.

- 명확하고 검증 가능한 Activation / Aha Moment 정의
- 세그먼트별 퍼널 및 리텐션 누수 정량화
- 통계적으로 타당한 실험 확대 적용 판단
- 기대 효과가 포함된 단일 우선 실행안 제시