# 한화솔루션 인허가 검토 프로그램

_Started 2026-06-30 01:26 UTC_

---

## User

# Claude Design 프롬프트
## 한화솔루션 인허가 검토 프로그램

---

## 제품 개요

**제품명:** 인허가 검토 프로그램  
**용도:** 석유화학공장 공정변경 법규 검토 보조 도구  
**사용자:** 한화솔루션 안전보건팀 인허가 검토 담당자  
**환경:** 데스크탑 브라우저 전용 (1280px 이상 기준)

---

## 디자인 원칙 (한화솔루션 브랜드 기반)

### 브랜드 컬러 시스템

한화솔루션 공식 회사소개 자료에서 도출한 컬러를 기준으로 한다.

```
Primary Orange   : #E87722  (한화솔루션 메인 오렌지, CTA 버튼·강조 요소)
Light Orange     : #F5A05A  (호버 상태·보조 강조)
Deep Orange      : #C45E10  (액티브 상태·테두리 강조)
Background White : #FFFFFF  (카드·패널 배경)
Page Background  : #F8F6F3  (전체 페이지 배경, 따뜻한 오프화이트)
Text Primary     : #1A1A1A  (본문 주요 텍스트)
Text Secondary   : #5C5C5C  (보조 텍스트·레이블)
Text Muted       : #9A9A9A  (힌트·비활성 텍스트)
Border Default   : #E8E4DE  (카드 테두리·구분선)
Border Strong    : #C8C2BA  (강조 구분선)

법령 카테고리 컬러 (뱃지용):
산안법   : #DC2626 배경 #FEF2F2  (위험·필수 의미)
고압가스법 : #D97706 배경 #FFFBEB (주의·경계 의미)
액화석유가스법 : #7C3AED 배경 #F5F3FF (특수·전문 의미)
에너지법 : #059669 배경 #ECFDF5  (안전·허가 의미)
화관법   : #2563EB 배경 #EFF6FF  (정보·참고 의미)
```

### 타이포그래피

- **영문 Display:** Pretendard 또는 system-ui, font-weight 600~700
- **본문:** Pretendard / system-ui, 14px, line-height 1.65, weight 400
- **UI 레이블:** 12px, weight 500, letter-spacing 0.02em
- **법조문 인용:** font-family monospace 계열, 13px, 배경 연파랑

### 레이아웃 원칙

1. **헤더 고정 네비게이션** — 좌측 한화솔루션 로고(오렌지) + 제품명, 우측 탭 메뉴(검토 요청·히스토리·법규 문서·공정 설정)
2. **2열 그리드 (검토 요청 화면)** — 좌 40% 입력 패널, 우 60% 결과 패널, 중간 1px 구분선
3. **카드 컴포넌트** — border-radius 10px, border 1px solid #E8E4DE, padding 20px 24px, 배경 흰색
4. **최소 여백** — 섹션 간 24px, 컴포넌트 내 16px, 인라인 요소 8px
5. **데스크탑 최적화** — min-width 1280px, 콘텐츠 최대 너비 1440px, 좌우 패딩 32px

### 시각적 아이덴티티

한화솔루션 브랜드 자료의 **웨이브 라인 그래픽**을 앱의 서명 요소로 활용한다.

- 헤더 우측 또는 빈 상태(empty state) 배경에 오렌지 계열 웨이브 SVG 장식 삽입
- 웨이브는 `opacity: 0.07~0.12` 수준의 배경 장식 — 콘텐츠를 방해하지 않는다
- 로딩·스트리밍 중 상태에서 웨이브 애니메이션(subtle pulse) 적용 가능

### UI 컴포넌트 스타일

**버튼**
- Primary: 배경 #E87722, 텍스트 흰색, border-radius 8px, padding 10px 20px, hover → #C45E10
- Ghost: 배경 투명, border 1px solid #E8E4DE, 텍스트 #5C5C5C, hover → 배경 #F8F6F3

**입력 필드**
- border 1px solid #E8E4DE, border-radius 8px, padding 10px 14px
- focus: border-color #E87722, box-shadow 0 0 0 3px rgba(232,119,34,0.12)

**뱃지 / 태그**
- 법령 카테고리별 고유 컬러 (위 컬러 시스템 참고)
- border-radius 20px (pill), font-size 11px, font-weight 600, padding 3px 10px

**공정 감지 배너**
- 배경 #FFF7ED (연한 오렌지), border-left 3px solid #E87722, border-radius 0 8px 8px 0
- 아이콘(번개 or 검색): 오렌지, 텍스트 12px

**법조문 인용 블록**
- 배경 #EFF6FF (연파랑), border-left 3px solid #2563EB
- 본문 폰트 13px, 출처 표기 10px 회색 소문자

**면책 문구 박스 (고정)**
- 배경 #FFFBEB, border 1px solid #F5A05A, border-radius 8px
- 아이콘: 삼각형 경고, 텍스트 13px #92400E

---

## 화면별 설계 명세

### 화면 A — 검토 요청 탭 (기본 화면, 가장 중요)

```
┌─────────────────────────────────────────────────────────────────────┐
│ 🔶 한화솔루션  인허가 검토    [검토 요청] [히스토리] [법규 문서] [공정 설정] │
├─────────────────────────────────────────────────────────────────────┤
│  ← 입력 패널 (40%) ──────────────┬── 결과 패널 (60%) ──────────────→ │
│                                  │                                  │
│  [레이블] 공정변경 문의 내용       │  [출처 기반] [PE공정] [고법] [산안]  │
│  ┌─────────────────────────┐     │                                  │
│  │ textarea (140px)        │     │  ## 검토 요약                    │
│  │ placeholder:            │     │  ...스트리밍 출력 중...           │
│  │ 문의 내용을 그대로        │     │                                  │
│  │ 붙여넣으세요.            │     │  > 법조문 인용 블록              │
│  └─────────────────────────┘     │  > 출처: 고압가스법 §5           │
│                                  │                                  │
│  ⚡ 공정 감지됨: PE공정 · VCM    │  1. 인허가 절차 1                │
│  우선 검토: 고법 · 산안 · 화관   │  2. 인허가 절차 2                │
│  [수정]                          │                                  │
│                                  │  ⚠ 본 결과는 참고용 초안입니다.  │
│  [설비명]    [처리물질]           │    최종 판단은 담당자 확인 후    │
│  [변경전용량] [변경후용량]        │    적용하십시오.                 │
│                                  │                                  │
│  [     법규 검토 시작 ▶     ]   │  [복사] [저장] [히스토리 등록]   │
│                                  │                                  │
└─────────────────────────────────────────────────────────────────────┘
```

**핵심 상태 3가지를 반드시 디자인:**
1. **대기 상태** — 입력 전, empty state에 웨이브 배경 장식 + "문의 내용을 입력하면 법규 검토가 시작됩니다" 안내
2. **스트리밍 중** — 결과 패널 상단에 오렌지 진행 인디케이터, 버튼이 [검토 중단]으로 교체
3. **완료 상태** — 법조문 인용 블록 + 절차 리스트 + 면책 박스 + 액션 버튼 표시

---

### 화면 B — 히스토리 탭

```
┌─────────────────────────────────────────────────────────────────────┐
│  [🔍 키워드 검색...]  [전체] [산안] [고법] [액법] [에너지] [화관]  총 12건 │
├─────────────────────────────────────────────────────────────────────┤
│ ┌──────────────────────────────────────────────────────────────┐    │
│ │ LPG 저장탱크 5톤 → 10톤 교체 인허가 검토          2025.06.12 │    │
│ │ 저장능력 변경에 따른 액화석유가스법 사업허가 변경 및 고압가스법  │    │
│ │ 완성검사 절차 안내. 한국가스안전공사 기술검토 신청 포함.        │    │
│ │ [액법] [고법] [PE공정]                     [열기] [삭제]    │    │
│ └──────────────────────────────────────────────────────────────┘    │
│ ┌──────────────────────────────────────────────────────────────┐    │
│ │ 반응기 안전밸브 용량 변경                           2025.05.28 │    │
│ │ ...                                                          │    │
│ └──────────────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────────────┘
```

---

### 화면 C — 법규 문서 탭

```
┌─────────────────────────────────────────────────────────────────────┐
│ ┌──────────────────────────────────────────────────────┐            │
│ │            PDF 파일을 여기에 드래그하거나             │            │
│ │              클릭하여 선택하세요                     │            │
│ │           (20MB 이하 · PDF만 허용)                  │            │
│ └──────────────────────────────────────────────────────┘            │
│                                                                     │
│  문서명              분류        등록일      페이지   활성   삭제     │
│  산업안전보건법.pdf   산안법     2025.01.15   89p   [ON]   [🗑]     │
│  고압가스법.pdf      고법       2024.11.02  124p   [ON]   [🗑]     │
│  액화석유가스법.pdf  액법       ⚠ 2024.01  203p   [ON]   [🗑]     │
│  ↑ ⚠ 180일 경과: 최신 법규 확인 권장                               │
└─────────────────────────────────────────────────────────────────────┘
```

---

### 화면 D — 공정 설정 탭

```
┌─────────────────────────────────────────────────────────────────────┐
│  공정-법규 매핑 설정       [+ 공정 추가]  [저장]  [초기값 복원]        │
├────────────┬──────────────────┬──────────────────┬──────────────────┤
│ 공정명     │ 감지 키워드       │ 주요 적용 법규    │ 필수 확인 항목   │
├────────────┼──────────────────┼──────────────────┼──────────────────┤
│ PE 공정    │ 에틸렌,LDPE,HDPE │ 고법특정,산안PSM  │ 특정제조허가,PSM │
│ VCM 공정   │ VCM,EDC,염화비닐 │ 고법특정,산안,화관│ 독성가스허가,PSM │
│ PVC 공정   │ PVC,현탁,반응기   │ 산안PSM,화관,에너지│ PSM검토,자체검사│
│ 가소제     │ 가소제,에스테르   │ 산안,에너지법     │ 보일러검사,화관  │
│ 유틸리티   │ LPG,스팀,보일러  │ 액법,에너지법     │ LPG허가,보일러   │
└────────────┴──────────────────┴──────────────────┴──────────────────┘
```

---

## 공통 UX 패턴

### 네비게이션 탭
- 활성 탭: 오렌지 하단 언더라인 2px + 텍스트 #E87722 + font-weight 600
- 비활성 탭: 텍스트 #9A9A9A, hover → #5C5C5C
- 탭 전환 시 URL hash 업데이트 (#review / #history / #docs / #settings)

### 빈 상태 (Empty State)
- 중앙 정렬, 웨이브 SVG 일러스트 (오렌지 톤, opacity 0.3)
- 제목 16px + 설명 14px 회색 + CTA 버튼

### 에러 상태
- 배경 #FEF2F2, 테두리 #F87171, 아이콘 빨간 X
- 에러 메시지: 무슨 문제가 생겼는지 + 어떻게 해결하는지 한 문장씩

### 로딩 / 스트리밍
- 오렌지 점 3개 bouncing 애니메이션 (dot-dot-dot)
- 결과 패널 상단에 "법규 검토 중..." 텍스트 + 진행 바

### 토스트 알림
- 우측 하단 고정, border-radius 8px, 배경 #1A1A1A, 텍스트 흰색
- 성공: 좌측 오렌지 바, 에러: 좌측 빨간 바
- 3초 후 자동 사라짐

---

## API Key 입력 모달 (첫 실행 시)

```
┌─────────────────────────────────┐
│  🔶 한화솔루션 인허가 검토 프로그램  │
│                                 │
│  Claude API Key를 입력하세요      │
│  ┌─────────────────────────┐   │
│  │ ••••••••••••••••••••••  │   │
│  └─────────────────────────┘   │
│  console.anthropic.com에서 발급  │
│                                 │
│       [  시작하기  ]            │
└─────────────────────────────────┘
```

- 배경 오버레이: rgba(0,0,0,0.5)
- 모달 배경: 흰색, border-radius 12px, 최대 너비 420px
- 모달 상단에 한화솔루션 오렌지 로고 또는 웨이브 장식
- API Key 없이 닫기 불가

---

## 한화솔루션 브랜드 요소 활용 가이드

| 요소 | 적용 위치 | 스타일 |
|------|-----------|--------|
| 오렌지 (#E87722) | CTA 버튼, 탭 활성, 강조 뱃지, 로고 | 메인 액션 유일하게 사용 |
| 웨이브 그래픽 | 헤더 배경 우측, 빈 상태 화면 | opacity 0.08~0.12, 오렌지 단색 |
| 따뜻한 오프화이트 (#F8F6F3) | 전체 페이지 배경 | 차갑지 않은 중립 배경 |
| 얇은 테두리 (1px) | 카드, 입력 필드 | 무겁지 않게, 섬세하게 |
| 법령 카테고리 컬러 | 뱃지, 필터 버튼 | 의미 기반 색상 배정 |

---

## 출력 요청

위 명세를 바탕으로 다음을 제작해 주세요:

1. **화면 A (검토 요청 탭)** — 가장 중점적으로 완성도 높게
   - 대기·스트리밍·완료 3가지 상태 모두 표현
   - 공정 감지 배너 포함
   - 결과 패널의 법조문 인용 블록·면책 박스 포함

2. **화면 B (히스토리 탭)** — 카드 목록 + 필터 바

3. **네비게이션 바** — 한화솔루션 브랜드 톤 반영

4. **API Key 모달** — 첫 실행 진입점

단일 HTML 파일 또는 Figma 프레임으로 제공.  
실제 작동하는 인터랙티브 프로토타입 형태 선호.
```

## Assistant

This is a thoroughly-specified prototype — the spec covers colors, type, every screen, and all states. I'll build it directly as an interactive Design Component rather than asking questions.

A note up front: I'll use the orange palette, wave motif, and "한화솔루션" wordmark exactly as your spec describes, but render the logo as a typographic mark + diamond (not an actual company logo file), since I shouldn't reproduce a real corporate logo.

Let me set up and build.

_[tool: update_todos]_

Building the full prototype as one Design Component.

_[tool: dc_write]_

Let me check it renders and step through the states.

_[tool: show_html]_

_[tool: show_html]_

_[tool: multi_screenshot]_

Modal and brand shell look right. Let me dismiss it and run through the review states.

_[tool: eval_js]_

_[tool: multi_screenshot]_

State resets between calls (iframe reload). Let me drive the whole flow in one sequence.

_[tool: multi_screenshot]_

The synthetic input events aren't updating React's controlled state. Let me use the native value setter.

_[tool: multi_screenshot]_

All three states work: idle → streaming with the orange progress bar and dot animation → done with citation blocks, badges, and procedure list. The brand shell, wave motif, process-detection banner, and structured fields all render correctly. Handing off.

_[tool: update_todos]_

_[tool: ready_for_verification]_

명세대로 인터랙티브 프로토타입을 완성했습니다.

**구현된 내용:**
- **API Key 모달** — 첫 진입 게이트, 한화솔루션 오렌지 로고 + 웨이브 장식
- **검토 요청 탭** — 입력 패널(40%) + 결과 패널(60%) 2열 그리드, 공정 감지 배너, 4개 구조화 필드. 결과 패널은 **대기 → 스트리밍 → 완료** 3개 상태 모두 작동 (스트리밍 진행 바·점 애니메이션·타이핑 캐럿, 완료 시 법조문 인용 블록·절차 리스트·면책 박스·액션 버튼)
- **히스토리 탭** — 카테고리 필터 + 키워드 검색 + 카드 목록 (실제 필터링 동작)
- **법규 문서 / 공정 설정 탭** — 업로드 영역, 문서 테이블(180일 경과 경고), 공정-법규 매핑 테이블
- 오렌지 브랜드 컬러·웨이브 모티프·법령 카테고리 뱃지 컬러·토스트 알림 적용

스트리밍 출력은 데모용 캔드(canned) 텍스트로 구현했습니다. 실제 Claude API 연동이 필요하면 알려주세요 — 그때 입력값 기반 실시간 검토로 연결하겠습니다.

## User

첨부 사진과 같은 부분에서 ▶를 지워주세요

## Assistant

_[tool: dc_html_str_replace]_

버튼의 ▶ 기호를 제거했습니다.

## User

공정변경 문의 내용 입력 칸을
초기 화면에서 나타나는 크기를 최대한으로 세로 길이를 늘려주세요. 한 화면을 넘어가지 않는 한에서

## Assistant

_[tool: str_replace_edit]_

_[tool: show_html]_

_[tool: multi_screenshot]_

입력 칸이 패널 높이에 맞춰 세로로 최대한 늘어났습니다.

## User

우측 상단의 가로로 표현된 4개 메뉴 버튼을
화면의 가장 좌측에 세로로 메뉴가 구현되도록 수정해주세요.

## Assistant

_[tool: read_file]_

_[tool: read_file]_

_[tool: read_file]_

Now restructuring to a left vertical sidebar. First the markup:

_[tool: str_replace_edit]_

Now close the content column before the toast, and update navStyle to vertical:

_[tool: str_replace_edit]_

_[tool: str_replace_edit]_

_[tool: show_html]_

_[tool: multi_screenshot]_

좌측 세로 사이드바로 메뉴를 옮겼습니다. 로고와 4개 메뉴(검토 요청·히스토리·법규 문서·공정 설정)가 화면 왼쪽에 세로로 배치되고, 활성 메뉴는 오렌지 강조와 왼쪽 액센트 바로 표시됩니다.

## User

"인허가 검토 프로그램" 문구를 위 "한화솔루션" 글자 좌측에 맞추어 이동해주세요.

## Assistant

_[tool: str_replace_edit]_

"인허가 검토 프로그램" 문구를 "한화솔루션" 글자 시작 위치에 맞춰 정렬했습니다.

## User

화면의 흰색 박스를 보이는 한 화면에 가득 차도록 확대해 주세요(빨간색 상자 크기 정도)

## Assistant

_[tool: str_replace_edit]_

_[tool: show_html]_

_[tool: save_screenshot]_

흰색 박스가 보이는 화면 높이에 가득 차도록 확대됐습니다. 입력칸도 늘어난 높이에 맞춰 함께 커집니다.

## User

"공정변경 문의 내용" 문구의 폰트가 작고, 일반텍스트로 구현되어 잘 보이지 않습니다. 디자인을 개선 해주세요.

## Assistant

_[tool: str_replace_edit]_

"공정변경 문의 내용" 라벨을 15px·굵게·진한 색으로 키우고, 왼쪽에 오렌지 액센트 바를 더해 섹션 제목처럼 또렷하게 만들었습니다.

## User

프로젝트 명 및 프로그램 내 문구를 수정해 주세요
인허가 검토 프로그램 → 법규 · 인허가 검토 프로그램

## Assistant

_[tool: grep]_

_[tool: str_replace_edit]_

사이드바 문구를 "법규 · 인허가 검토 프로그램"으로 수정했습니다.

