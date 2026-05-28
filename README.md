# 김비서 AI 워크숍 — 전체 진행 가이드

> 이 문서는 워크숍에서 진행한 모든 작업을 순서대로 정리한 재현 가이드입니다.  
> 처음부터 따라 하면 동일한 결과물을 만들 수 있습니다.

---

## 목차

1. [최종 결과물 구조](#1-최종-결과물-구조)
2. [Step 1 — 데이터 파일 준비](#2-step-1--데이터-파일-준비)
3. [Step 2 — 메인 페이지 제작](#3-step-2--메인-페이지-제작-indexhtml)
4. [Step 3 — 대시보드 제작](#4-step-3--대시보드-제작-dashboardhtml)
5. [Step 4 — 프리미엄 디자인 업그레이드](#5-step-4--프리미엄-디자인-업그레이드)
6. [Step 5 — 회의록 페이지 제작](#6-step-5--회의록-페이지-제작-meeting-resulthtml)
7. [Step 6 — 파일 분류 정리](#7-step-6--파일-분류-정리)
8. [Step 7 — 매출 차트 제작](#8-step-7--매출-차트-제작-charthtml)
9. [Step 8 — 업무 프로세스 다이어그램 제작](#9-step-8--업무-프로세스-다이어그램-제작-diagramsvg)
10. [Step 9 — 사이트 분석 리포트 제작](#10-step-9--사이트-분석-리포트-제작-reporthtml)
11. [Step 10 — 전체 네비게이션 연결](#11-step-10--전체-네비게이션-연결)
12. [Step 11 — GitHub 업로드](#12-step-11--github-업로드)
13. [핵심 기술 레퍼런스](#13-핵심-기술-레퍼런스)

---

## 1. 최종 결과물 구조

```
워크숍-실습/
├── index.html              ← 김비서 소개 메인 페이지
├── dashboard.html          ← 업무 대시보드 (4개 섹션)
├── meeting-result.html     ← 회의록 리포트
├── chart.html              ← 매출 데이터 차트
├── diagram.svg             ← 업무 프로세스 다이어그램 (SVG)
├── diagram.html            ← 다이어그램 뷰어 (글래스 디자인)
├── report.html             ← 사이트 분석 리포트
├── .env.local              ← 환경변수 (GitHub 토큰, Git에 올리지 않음)
├── .gitignore              ← Git 제외 파일 목록
├── README.md               ← 이 문서
├── 김비서-데이터/
│   ├── 매출데이터.csv
│   ├── 업무목록.csv
│   ├── 주간일정.txt
│   ├── 프로젝트현황.csv
│   └── 회의록.txt
└── 정리해줘/
    ├── 보고서/
    ├── 메모/
    ├── 업무/
    └── 기타/
```

---

## 2. Step 1 — 데이터 파일 준비

### 사용한 데이터 파일 목록

| 파일명 | 형식 | 내용 |
|--------|------|------|
| 매출데이터.csv | CSV | 날짜·제품별 매출 (일별 10행) |
| 업무목록.csv | CSV | 할 일 목록 (담당자·우선순위·상태·기한) |
| 주간일정.txt | TXT | 월~금 요일별 일정 |
| 프로젝트현황.csv | CSV | 프로젝트 6개 + 진행률 |
| 회의록.txt | TXT | 회의 일시·참석자·논의 내용·액션 아이템 |

### CSV 포맷 예시 (업무목록.csv)

```
업무명,담당자,우선순위,상태,마감일
3월 프로모션 기획안 작성,김대리,높음,진행중,2026-03-12
인플루언서 미팅 일정 잡기,이과장,높음,대기,2026-03-14
```

---

## 3. Step 2 — 메인 페이지 제작 (index.html)

### 역할

김비서 서비스를 소개하는 랜딩 페이지. dashboard.html로 진입하는 관문.

### 핵심 구성 요소

```
[배경] 그라디언트 애니메이션 + 3개 블롭(blob)
[헤더] 로고 + 테마 토글 버튼 (우측 상단 고정)
[히어로 카드] 글래스 카드 — 제목 + 설명 + CTA 버튼
[CTA] → dashboard.html 링크
```

### 그라디언트 배경 코드

```css
.bg {
  position: fixed; inset: 0; z-index: -2;
  background: linear-gradient(-45deg, #667eea, #764ba2, #f093fb, #4facfe, #00f2fe);
  background-size: 400% 400%;
  animation: gradShift 18s ease infinite;
}

@keyframes gradShift {
  0%   { background-position: 0% 50%; }
  50%  { background-position: 100% 50%; }
  100% { background-position: 0% 50%; }
}
```

### 블롭(Blob) 애니메이션 코드

```css
.blob {
  position: fixed; z-index: -1; border-radius: 50%;
  filter: blur(72px); opacity: .45;
  animation: blobFloat 12s ease-in-out infinite;
}
.blob-1 { width:520px; height:520px; background:#a78bfa; top:-120px; left:-100px; }
.blob-2 { width:420px; height:420px; background:#38bdf8; bottom:-80px; right:-80px; animation-delay:-5s; }
.blob-3 { width:360px; height:360px; background:#f472b6; top:40%; left:60%; animation-delay:-9s; }

@keyframes blobFloat {
  0%,100% { transform: translate(0,0) scale(1); }
  33%      { transform: translate(30px,-40px) scale(1.06); }
  66%      { transform: translate(-20px,30px) scale(.96); }
}
```

---

## 4. Step 3 — 대시보드 제작 (dashboard.html)

### 역할

업무 데이터를 4개 섹션으로 시각화하는 핵심 페이지.

### 4개 섹션 구성

| 섹션 | 데이터 출처 | 기능 |
|------|------------|------|
| ① 할 일 목록 | 업무목록.csv | 클릭 시 완료 토글, 우선순위·상태 배지 |
| ② 이번 주 일정 | 주간일정.txt | 요일별 이벤트 카드 |
| ③ 프로젝트 진행률 | 프로젝트현황.csv | 진행 바(progress bar) |
| ④ 매출 요약 | 매출데이터.csv | 핵심 지표 4개 + 수평 막대 차트 |

### CSS 그리드 레이아웃

```css
.grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 20px;
}

/* 모바일 반응형 */
@media (max-width: 860px) {
  .grid { grid-template-columns: 1fr; }
}
```

### 데이터를 JS에 하드코딩하는 방법

CSV 파일을 읽어서 필요한 값을 직접 JS 배열로 작성합니다:

```javascript
const tasks = [
  { name:'3월 프로모션 기획안 작성', priority:'높음', status:'진행중', due:'3/12', assignee:'김대리' },
  { name:'인플루언서 미팅 일정 잡기', priority:'높음', status:'대기',   due:'3/14', assignee:'이과장' },
  // ...
];
```

---

## 5. Step 4 — 프리미엄 디자인 업그레이드

두 페이지(index.html, dashboard.html)를 **글래스모피즘** 디자인으로 업그레이드 + **다크/라이트 모드** 추가.

### 글래스모피즘 카드 CSS

```css
.card {
  background: rgba(255,255,255,0.58);
  border: 1px solid rgba(255,255,255,0.75);
  box-shadow: 0 8px 32px rgba(100,116,139,.13);
  backdrop-filter: blur(18px);
  -webkit-backdrop-filter: blur(18px);
  border-radius: 20px;
  padding: 24px;
}
```

### CSS 변수 시스템 (테마 분기)

```css
/* 라이트 모드 기본값 */
:root {
  --glass-bg:     rgba(255,255,255,0.58);
  --glass-border: rgba(255,255,255,0.75);
  --text-1: #1e293b;
  --text-2: #475569;
  --text-3: #94a3b8;
  --accent: #6366f1;
  --surface: rgba(255,255,255,0.45);
  --surface-border: rgba(100,116,139,.15);
}

/* 다크 모드 덮어쓰기 */
[data-theme="dark"] {
  --glass-bg:     rgba(30,27,75,0.55);
  --glass-border: rgba(99,102,241,0.18);
  --text-1: #e2e8f0;
  --text-2: #94a3b8;
  --text-3: #64748b;
  --accent: #818cf8;
  --surface: rgba(99,102,241,0.08);
}
```

### 다크 모드 토글 JavaScript

```javascript
const html = document.documentElement;
const btn  = document.getElementById('themeToggle');

function applyTheme(t) {
  html.setAttribute('data-theme', t);       // <html data-theme="dark">
  btn.textContent = t === 'dark' ? '☀️' : '🌙';
}

// 페이지 로드 시 저장된 테마 복원
applyTheme(localStorage.getItem('kim-theme') || 'light');

// 버튼 클릭 시 토글 + 저장
btn.addEventListener('click', () => {
  const next = html.getAttribute('data-theme') === 'dark' ? 'light' : 'dark';
  localStorage.setItem('kim-theme', next);  // 두 페이지 공통 키 'kim-theme'
  applyTheme(next);
});
```

> **핵심:** `localStorage` 키를 `'kim-theme'`으로 통일하면 index.html ↔ dashboard.html 사이에서 테마가 유지됩니다.

---

## 6. Step 5 — 회의록 페이지 제작 (meeting-result.html)

### 역할

`회의록.txt` 내용을 인쇄 가능한 HTML 문서로 변환.

### 구성 요소

```
[헤더] 인디고 그라디언트 배너 (일시·장소·참석자·작성자)
[섹션 1] 논의 내용 요약 — 번호 배지 + 좌측 보더 카드
[섹션 2] 액션 아이템 — 담당자 칩 + 기한 색상 배지 표
[푸터] 다음 회의 일정
[버튼] 인쇄 / PDF 저장
```

### 인쇄 스타일 설정

```css
@media print {
  body { background: white; padding: 0; }
  .page { max-width: 100%; border-radius: 0; box-shadow: none; }
  .print-btn { display: none; }   /* 인쇄 시 버튼 숨기기 */

  /* 배경색도 인쇄되게 강제 */
  .page-header {
    -webkit-print-color-adjust: exact;
    print-color-adjust: exact;
  }

  @page { margin: 16mm 14mm; size: A4; }
}
```

### 인쇄 버튼

```html
<button onclick="window.print()">🖨️ 인쇄 / PDF 저장</button>
```

---

## 7. Step 6 — 파일 분류 정리

### 정리 전 상태

`정리해줘/` 폴더에 15개 파일이 뒤섞여 있음.

### 분류 기준

| 폴더 | 해당 파일 |
|------|---------|
| `보고서/` | 보고서_초안, 보고서_수정, 보고서_최종, 보고서_최종_진짜최종 |
| `메모/` | 기획_메모, 메모, 아이디어_메모_0301, 나중에정리, 무제 |
| `업무/` | 할일, 영수증_정리, 피드백, 회의실 |
| `기타/` | temp.csv, 링크모음 |

### PowerShell 명령어 (Windows)

```powershell
# 하위 폴더 생성
New-Item -ItemType Directory "정리해줘\보고서", "정리해줘\메모", "정리해줘\업무", "정리해줘\기타"

# 파일 이동
Move-Item "정리해줘\보고서_*.txt" "정리해줘\보고서\"
Move-Item "정리해줘\*메모*.txt"   "정리해줘\메모\"
# ...
```

---

## 8. Step 7 — 매출 차트 제작 (chart.html)

### 역할

`매출데이터.csv`를 **외부 라이브러리 없이** Canvas API로 시각화.

### 차트 종류

| 차트 | 설명 |
|------|------|
| 선 그래프 | 날짜별 매출 추이, 베지어 곡선 스무딩, 그라디언트 채우기 |
| 수평 막대 그래프 | 제품별 매출 비교, 초록 그라디언트, 1위 배지 |

### Canvas 고해상도(레티나) 처리

```javascript
function initCanvas(canvas) {
  const dpr  = window.devicePixelRatio || 1;
  const rect = canvas.getBoundingClientRect();

  canvas.width  = rect.width  * dpr;
  canvas.height = rect.height * dpr;

  const ctx = canvas.getContext('2d');
  ctx.scale(dpr, dpr);           // 좌표계는 CSS 픽셀로 유지
  return ctx;
}
```

### 베지어 곡선 스무딩

```javascript
function smoothLine(ctx, points) {
  ctx.beginPath();
  ctx.moveTo(points[0].x, points[0].y);
  for (let i = 1; i < points.length - 1; i++) {
    const cpX = (points[i].x + points[i+1].x) / 2;
    const cpY = (points[i].y + points[i+1].y) / 2;
    ctx.quadraticCurveTo(points[i].x, points[i].y, cpX, cpY);
  }
  ctx.lineTo(points.at(-1).x, points.at(-1).y);
}
```

### 리사이즈 디바운스

```javascript
window.addEventListener('resize', () => {
  clearTimeout(window._rt);
  window._rt = setTimeout(render, 150);   // 150ms 대기 후 재렌더
});
```

### 숫자 한국어 포맷

```javascript
value.toLocaleString('ko-KR')  // 7101 → "7,101"
```

---

## 9. Step 8 — 업무 프로세스 다이어그램 제작 (diagram.svg)

### 규격

- 크기: 800 × 200px
- 형식: 순수 SVG (HTML 불필요, 브라우저에서 직접 열림)

### 레이아웃 계산

```
박스 너비 120px, 간격(화살표) 44px, 좌우 여백 24px
24 + (120 + 44) × 4 + 120 + 24 = 800px ✓

박스 x 좌표: 24 / 188 / 352 / 516 / 680
중심 cx 좌표: 84 / 248 / 412 / 576 / 740
```

### 단계별 색상

| 단계 | 배경 | 배지 색 |
|------|------|--------|
| 기획 | `#DBEAFE` | `#2563EB` |
| 제작 | `#EDE9FE` | `#7C3AED` |
| 검토 | `#FEF3C7` | `#D97706` |
| 배포 | `#D1FAE5` | `#059669` |
| 분석 | `#FCE7F3` | `#BE185D` |

### SVG 드롭섀도 필터

```xml
<defs>
  <filter id="shadow" x="-15%" y="-25%" width="130%" height="160%">
    <feDropShadow dx="0" dy="3" stdDeviation="4"
                  flood-color="#94A3B8" flood-opacity="0.18"/>
  </filter>
</defs>
```

### 숫자 배지 (박스 상단 중앙에 반쯤 걸치게)

```xml
<!-- 박스 y=64, 배지 r=13 → 배지 cy=64 (박스 상단 라인에 걸침) -->
<rect x="24" y="64" width="120" height="72" rx="14" .../>
<circle cx="84" cy="64" r="13" fill="#2563EB"/>
<text x="84" y="68.8" text-anchor="middle" fill="white">1</text>
```

---

## 10. Step 9 — 사이트 분석 리포트 제작 (report.html)

### 역할

외부 웹사이트를 분석해서 구조·디자인·평가를 카드 형태로 정리.

### 구성 섹션

```
① 기본 정보 (URL, 유형, 대상 사용자)
② 핵심 기능 (기능 배지 목록)
③ 구조 & 디자인 분석
④ 잘한 점 / 개선 필요 사항 (초록/주황 border 카드)
⑤ 총평 (별점 + 한 줄 요약)
```

### 평가 카드 스타일

```css
.eval-card.good    { border-left: 4px solid #10B981; background: #F0FDF4; }
.eval-card.improve { border-left: 4px solid #F59E0B; background: #FFFBEB; }
```

> **주의:** JavaScript SPA(React, Vue 등)로 만든 사이트는 정적 크롤링으로 내용이 안 잡힙니다.  
> 이 경우 App Store 리뷰, Hacker News 등 외부 정보를 활용하고  
> 리포트에 "SPA 한계로 정적 분석" 안내 배너를 추가합니다.

---

## 11. Step 10 — 전체 네비게이션 연결

모든 페이지를 하나의 탭 메뉴로 연결. 현재 페이지 탭은 자동 강조.

### dashboard.html 전용 nav (글래스 스타일)

`</header>` 바로 아래에 삽입:

```html
<nav class="nav" id="mainNav">
  <a href="dashboard.html"      class="nav-tab" data-page="dashboard">📊 대시보드</a>
  <a href="meeting-result.html" class="nav-tab" data-page="meeting-result">📝 회의록</a>
  <a href="chart.html"          class="nav-tab" data-page="chart">📈 매출 현황</a>
  <a href="diagram.html"        class="nav-tab" data-page="diagram">🔄 업무 프로세스</a>
  <a href="report.html"         class="nav-tab" data-page="report">🔍 사이트 분석</a>
</nav>
```

```css
.nav {
  background: var(--header-bg); backdrop-filter: blur(22px);
  border-bottom: 1px solid var(--header-border);
  height: 46px; position: sticky; top: 60px; z-index: 99;
  display: flex; align-items: center; gap: 3px; padding: 0 32px;
  overflow-x: auto; scrollbar-width: none;
}
.nav-tab { padding: 5px 13px; border-radius: 8px; text-decoration: none; ... }
.nav-tab.active { background: var(--badge-bg); color: var(--accent); }
```

### 다른 HTML 페이지용 nav (고정 흰색 바)

`<body>` 태그 바로 아래에 삽입. body의 `padding-top`을 기존 값 + 46px로 늘림:

```html
<nav class="site-nav">
  <!-- 동일한 5개 링크 -->
</nav>
```

```css
.site-nav {
  position: fixed; top: 0; left: 0; right: 0; z-index: 200;
  height: 46px; background: rgba(255,255,255,0.92);
  backdrop-filter: blur(16px);
  border-bottom: 1px solid #E2E8F0;
  display: flex; align-items: center; gap: 3px; padding: 0 24px;
}
/* 기존 body padding-top에 46px 추가 */
body { padding-top: 64px; /* 원래 48px → 48+16 = 64px */ }
```

### 현재 페이지 탭 자동 강조 JS

모든 페이지 `<script>` 최상단에 추가:

```javascript
(function() {
  // URL에서 파일명 추출 (확장자 제거)
  const page = location.pathname.split('/').pop().replace(/\.[^.]+$/, '') || 'dashboard';
  document.querySelectorAll('.nav-tab, .site-nav-tab').forEach(a => {
    if (a.dataset.page === page) a.classList.add('active');
  });
})();
```

### diagram.html — SVG 전용 글래스 래퍼

`diagram.svg`는 nav를 직접 삽입할 수 없으므로 HTML 래퍼 파일을 별도 생성:

```html
<!-- diagram.html -->
<img src="diagram.svg" alt="업무 프로세스 다이어그램">
```

---

## 12. Step 11 — GitHub 업로드

### 사전 준비

1. GitHub에서 빈 repository 생성 (예: `santa`)
2. GitHub Personal Access Token 발급  
   `Settings → Developer settings → Personal access tokens → Generate new token`  
   권한: `repo` (전체) 체크

### .env.local 파일 생성

```
GITHUB_TOKEN=ghp_여기에토큰붙여넣기
```

### .gitignore 파일 생성 (필수)

```gitignore
.env
.env.local
.env*.local
.claude/
```

> ⚠️ **반드시 `git add` 전에 .gitignore를 만들어야 합니다.**  
> 나중에 추가하면 이미 추적된 파일은 계속 올라갑니다.

### Git 초기화 및 업로드 명령어

```bash
# 1. Git 저장소 초기화
git init

# 2. 사용자 정보 설정 (최초 1회)
git config user.name "깃허브아이디"
git config user.email "아이디@users.noreply.github.com"

# 3. 파일 전체 스테이징
git add .

# 4. 커밋
git commit -m "Initial commit: 프로젝트명"

# 5. 브랜치 이름 main으로 변경
git branch -M main

# 6. 토큰을 사용해서 push (토큰은 URL에 직접 삽입)
TOKEN=$(grep GITHUB_TOKEN .env.local | cut -d= -f2)
git push "https://깃허브아이디:${TOKEN}@github.com/깃허브아이디/레포이름.git" main
```

### gitignore 재적용 (이미 커밋한 파일 제거)

```bash
# 인덱스(캐시) 전체 초기화 후 재add
git rm -r --cached .
git add .
git commit -m "Apply .gitignore"
git push
```

---

## 13. 핵심 기술 레퍼런스

### 디자인 시스템 요약

| 항목 | 라이트 | 다크 |
|------|--------|------|
| 배경 | 그라디언트 애니메이션 | `#0f0f23` 계열 어두운 그라디언트 |
| 카드 | `rgba(255,255,255,0.58)` | `rgba(30,27,75,0.55)` |
| 텍스트 | `#1e293b` | `#e2e8f0` |
| 강조색 | `#6366f1` (인디고) | `#818cf8` (연한 인디고) |
| 배경 블러 | `backdrop-filter: blur(18px)` | 동일 |

### 자주 쓰는 CSS 패턴

```css
/* 글래스 카드 */
.card {
  background: var(--glass-bg);
  border: 1px solid var(--glass-border);
  backdrop-filter: blur(18px);
  -webkit-backdrop-filter: blur(18px);  /* Safari 호환 */
  border-radius: 20px;
}

/* 스크롤바 숨기기 */
.scrollable { overflow-y: auto; scrollbar-width: none; }
.scrollable::-webkit-scrollbar { display: none; }

/* 카드 등장 애니메이션 */
.card { animation: fadeUp .5s ease both; }
@keyframes fadeUp {
  from { opacity: 0; transform: translateY(20px); }
  to   { opacity: 1; transform: translateY(0); }
}

/* 스태거(순서차 지연) */
.card:nth-child(1) { animation-delay: .05s; }
.card:nth-child(2) { animation-delay: .15s; }
.card:nth-child(3) { animation-delay: .25s; }
```

### 배지 색상 패턴

```css
/* 우선순위 */
.p-high { background: rgba(220,38,38,.12);  color: #EF4444; }
.p-mid  { background: rgba(245,158,11,.12); color: #F59E0B; }
.p-low  { background: rgba(16,185,129,.12); color: #10B981; }

/* 상태 */
.s-done { background: rgba(16,185,129,.12); color: #10B981; }
.s-wip  { background: rgba(59,130,246,.12); color: #3B82F6; }
.s-wait { background: var(--surface-border); color: var(--text-3); }
```

### 프로젝트 확장 아이디어

다음 워크숍에서 추가할 수 있는 기능들:

- [ ] `index.html`에서도 nav 탭 표시
- [ ] 대시보드에 실시간 시계 추가
- [ ] chart.html에 날짜 필터 (기간 선택)
- [ ] meeting-result.html PDF 자동 저장 기능
- [ ] GitHub Pages로 무료 배포 (`Settings → Pages → main 브랜치`)
- [ ] 다국어 지원 (한국어 / 영어 전환)

---

*작성: 김비서 AI 워크숍 — 2026년 5월*
