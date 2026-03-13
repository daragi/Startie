# Repository Structure Spec

## 1) 문서 목적
이 문서는 레포의 현재 구조를 Git 관점에서 상세하게 설명하는 기준 문서다.
- 어떤 폴더에 무엇이 있는지
- 각 파일이 어떤 역할을 하는지
- 결과물과 원천 데이터가 어디에 있는지

기준일: `2026-03-13`

---

## 2) 루트 구조

```text
PDA/
├─ data/
├─ image/
├─ notebook/
├─ outputs/
├─ overview/
├─ pda/
├─ table/
├─ .gitignore
└─ README.md
```

---

## 3) 디렉터리 상세

### 3.1 `data/` (원천 데이터)
분석에 사용되는 CSV 원본/로그/실험 데이터.

```text
data/
├─ ab_assignment.csv
├─ campaigns.csv
├─ chat_events.csv
├─ courses.csv
├─ event_logs.csv
├─ experiments.csv
├─ join.csv
├─ lessons.csv
├─ payment_transactions.csv
├─ plan_history.csv
├─ push_events.csv
├─ referral_events.csv
├─ retention_action_rates_24h.csv
├─ retention_action_rates_24h_en.csv
└─ users.csv
```

주요 파일 역할
- `users.csv`: 사용자 마스터(가입/프로필/상태)
- `event_logs.csv`: 사용자 행동 이벤트 로그
- `payment_transactions.csv`: 결제 트랜잭션 로그
- `plan_history.csv`: 구독 플랜 변경 이력
- `experiments.csv`, `ab_assignment.csv`: 실험 메타/배정 정보

---

### 3.2 `notebook/` (분석 실행 노트북)
KPI, 퍼널, 행동 분석용 Jupyter 노트북.

```text
notebook/
├─ kpi_def.ipynb
├─ main.ipynb
├─ PDA_funnel.ipynb
└─ User_Behavior.ipynb
```

권장 해석
- `kpi_def.ipynb`: 지표 정의/기초 계산
- `PDA_funnel.ipynb`: 퍼널 단계 및 병목 분석
- `User_Behavior.ipynb`: 행동 기반 전환 분석
- `main.ipynb`: 종합 실행 또는 통합 정리

---

### 3.3 `overview/` (프로젝트 명세 문서)
프로젝트 설명, 데이터 사전, 분석 가이드, ERD, 경영진 질문 프레임.

```text
overview/
├─ 00 _startie_overview.md
├─ 01_data_dictionary.md
├─ 02_analysis_guide.md
├─ 03_erd_startie.dbml
└─ 04_ceo_questions.md
```

파일 역할
- `00 _startie_overview.md`: 서비스/비즈니스 배경
- `01_data_dictionary.md`: 테이블/컬럼 설명
- `02_analysis_guide.md`: 분석 절차/지표 운영 가이드
- `03_erd_startie.dbml`: ERD 스키마 정의
- `04_ceo_questions.md`: 의사결정 질문 중심 분석 프레임

---

### 3.4 `pda/` (프레임워크/실험 설계 문서)

```text
pda/
├─ AARRR/
│  ├─ AARRR_Framework.md
│  └─ AARRR_Framework.pdf
└─ A_B_Test/
   ├─ final_test_설계.md
   └─ 행동지표 A_B_Test.md
```

파일 역할
- `AARRR_Framework.*`: AARRR 기반 문제 구조화
- `A_B_Test/*`: 액션 검증용 A/B 테스트 설계서

---

### 3.5 `outputs/` (최종 산출물)
발표/보고용 최종 결과물 저장 경로.

```text
outputs/
├─ DA_results/
└─ ppt/
   ├─ Startie_PDA.pdf
   └─ startie_presentation_flow.md
```

파일 역할
- `DA_results/`: 핵심 분석 차트(최종 사용본)
- `ppt/Startie_PDA.pdf`: 발표용 최종 PDF
- `ppt/startie_presentation_flow.md`: 보고서형 발표 흐름 문서

---

### 3.6 `image/` (보조 시각화 자산)
분석 과정 및 카테고리별 추가 차트.

```text
image/
├─ Aha/
├─ Bounce/
├─ CVR/
├─ funnel/
└─ Retention/
```

---

### 3.7 `table/` (중간 집계 테이블)
노트북 분석 과정에서 생성된 특징/집계 테이블.

```text
table/
├─ conversion_rate_by_completion.csv
├─ conversion_rate_by_count.csv
├─ conversion_rate_by_time.csv
└─ user_behavior_features.csv
```

---

## 4) Git 운영 기준 (권장)
- `data/`는 원천 데이터, `outputs/`는 최종 산출물로 역할 분리 유지
- 노트북 결과를 문서화할 때 `outputs/DA_results` 기준으로 참조 경로 통일
- 파일명 규칙은 가능하면 일관 유지
  - 한글/영문 혼용 최소화
  - 오타(예: `전화율`)는 추후 정리 시 일괄 교정
- 구조 변경 시 이 문서(`overview/05_repository_structure.md`)를 함께 갱신

---

## 5) 빠른 확인 명령어
아래 명령어로 구조를 최신 상태로 점검할 수 있다.

```powershell
# 추적 대상 파일 목록 확인
rg --files | Sort-Object

# data 파일 목록
Get-ChildItem -Path data -File | Select-Object Name | Sort-Object Name

# outputs 최종 산출물 확인
Get-ChildItem -Path outputs -Recurse -File | Select-Object FullName
```
