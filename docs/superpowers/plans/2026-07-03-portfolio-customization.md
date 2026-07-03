# 개인 포트폴리오 커스터마이징 Implementation Plan

**Goal:** INU 강의 스타터킷의 nav를 Project/CS/About/Components 4탭 구조로 바꾸고, Project 탭 카드를 실제 활동으로, CS 탭(신규)을 수강 과목 카드로, About 탭 본문을 자기소개로 교체한다.

**Architecture:** 빌드 없는 정적 HTML/CSS/JS 사이트. 페이지별 콘텐츠는 각 `.html` 파일에 하드코딩하고, nav/footer 공통 마크업만 `js/layout.js`가 주입한다. 기존 `.card`/`.chip`/`.container-grid`/`.container-text` 컴포넌트(css/components.css)를 그대로 재사용하며 새 CSS는 추가하지 않는다.

**Tech Stack:** HTML5, CSS3(커스텀 프로퍼티), 바닐라 JavaScript. 테스트 프레임워크 없음(빌드 단계 없는 정적 사이트) — 검증은 로컬 HTTP 서버 구동 후 브라우저 수동 확인으로 수행한다.

## Global Constraints

- 새 hex/px 인라인 금지 — 모든 스타일은 `var(--token)` 참조 (스펙: "모든 색상·간격·폰트는 css/tokens.css 변수만 참조")
- 새 CSS 클래스 추가 금지 — `.card`, `.chip`, `.container-grid`, `.container-text`만 재사용
- 경로는 항상 상대 경로(앞에 `/` 없이) — CLAUDE.md 코딩 컨벤션
- 필터칩 클릭 시 실제 카드 필터링 JS는 추가하지 않는다 — 스펙 "범위 밖" 항목
- 디자인 토큰, 브랜드 워드마크(`inu·starter`), `components.html`은 변경하지 않는다

---

### Task 1: nav/footer 탭 구조 변경 (js/layout.js)

**Files:**
- Modify: `js/layout.js:21-43` (`NAV_LINKS`, `FOOTER_COLUMNS`)

**Interfaces:**
- Consumes: 없음 (독립적인 첫 작업)
- Produces: 모든 페이지의 nav/footer에 반영되는 `NAV_LINKS`/`FOOTER_COLUMNS` — Task 2~4에서 만들 `cs.html`이 여기 등록된 링크로 접근 가능해야 함

- [ ] **Step 1: NAV_LINKS를 4탭 구조로 수정**

`js/layout.js`의 기존 코드:

```js
const NAV_LINKS = [
  { href: "index.html", label: "Home" },
  { href: "about.html", label: "About" },
  { href: "components.html", label: "Components" },
];
```

다음으로 교체:

```js
const NAV_LINKS = [
  { href: "index.html", label: "Project" },
  { href: "cs.html", label: "CS" },
  { href: "about.html", label: "About" },
  { href: "components.html", label: "Components" },
];
```

- [ ] **Step 2: FOOTER_COLUMNS의 "프로젝트" 컬럼 동기화**

기존 코드:

```js
const FOOTER_COLUMNS = [
  {
    heading: "프로젝트",
    links: [
      { href: "index.html", label: "홈" },
      { href: "about.html", label: "소개" },
      { href: "components.html", label: "컴포넌트" },
    ],
  },
  {
    heading: "리소스",
    links: [
      { href: "https://pages.github.com/", label: "GitHub Pages" },
      { href: "https://developer.mozilla.org/ko/", label: "MDN 문서" },
    ],
  },
];
```

다음으로 교체:

```js
const FOOTER_COLUMNS = [
  {
    heading: "프로젝트",
    links: [
      { href: "index.html", label: "프로젝트" },
      { href: "cs.html", label: "CS" },
      { href: "about.html", label: "소개" },
      { href: "components.html", label: "컴포넌트" },
    ],
  },
  {
    heading: "리소스",
    links: [
      { href: "https://pages.github.com/", label: "GitHub Pages" },
      { href: "https://developer.mozilla.org/ko/", label: "MDN 문서" },
    ],
  },
];
```

- [ ] **Step 3: 수동 검증**

`cs.html`이 아직 없으므로 이 시점에는 nav에 CS 링크가 떠도 404가 정상이다(Task 3에서 해소됨). 지금은 문법 오류가 없는지만 확인:

Run: `node --check js/layout.js`
Expected: 출력 없음(에러 없이 종료)

- [ ] **Step 4: Commit**

```bash
git add js/layout.js
git commit -m "feat: rename Home tab to Project, add CS tab to nav/footer"
```

---

### Task 2: index.html — Project 탭 콘텐츠 교체

**Files:**
- Modify: `index.html` (title/meta, hero, filter chips, card grid)

**Interfaces:**
- Consumes: Task 1의 `NAV_LINKS`(자동 주입되므로 이 파일에서 직접 참조하지 않음)
- Produces: 없음 (터미널 페이지)

- [ ] **Step 1: title/meta 변경**

기존:

```html
<title>INU Starter — Home</title>
<meta
  name="description"
  content="INU 강의용 github.io 정적 웹서비스 스타터킷"
/>
```

교체:

```html
<title>INU Starter — Project</title>
<meta
  name="description"
  content="이용진의 학기 중 프로젝트 및 활동 소개"
/>
```

- [ ] **Step 2: 히어로 문구 교체**

기존:

```html
<section class="hero">
  <div class="hero__inner">
    <p class="t-eyebrow">INU Lecture Starter Kit</p>
    <h1 class="t-hero-display">
      강의 프로젝트를 <span class="hero-accent">여기서</span> 시작하세요
    </h1>
    <p class="t-lead">
      순수 HTML · CSS · JS. 빌드 없이 github.io 에 바로 배포됩니다.
    </p>
    <div class="hero__cta">
      <a class="btn btn--primary btn--lg" href="components.html"
        >컴포넌트 보기</a
      >
      <a class="btn btn--lg" href="about.html">소개</a>
    </div>
  </div>
</section>
```

교체:

```html
<section class="hero">
  <div class="hero__inner">
    <p class="t-eyebrow">Projects</p>
    <h1 class="t-hero-display">
      학기 중 진행한 <span class="hero-accent">프로젝트</span>를 소개합니다
    </h1>
    <p class="t-lead">
      개발과 연구, 두 방향으로 진행한 활동들을 정리했습니다.
    </p>
    <div class="hero__cta">
      <a class="btn btn--primary btn--lg" href="components.html"
        >컴포넌트 보기</a
      >
      <a class="btn btn--lg" href="about.html">소개</a>
    </div>
  </div>
</section>
```

- [ ] **Step 3: 필터칩 교체**

기존:

```html
<div class="chip-row" style="margin-bottom: var(--space-8)">
  <button class="chip is-active" type="button">전체</button>
  <button class="chip" type="button">기초</button>
  <button class="chip" type="button">컴포넌트</button>
  <button class="chip" type="button">배포</button>
</div>
```

교체:

```html
<div class="chip-row" style="margin-bottom: var(--space-8)">
  <button class="chip is-active" type="button">전체</button>
  <button class="chip" type="button">개발</button>
  <button class="chip" type="button">연구</button>
</div>
```

- [ ] **Step 4: 카드 그리드를 6개 데모 카드 → 2개 프로젝트 카드로 교체**

기존 `<div class="card-grid">...</div>` 블록(6개 `<article class="card">`) 전체를 다음으로 교체:

```html
<div class="card-grid">
  <article class="card">
    <p class="card__tag">Dev</p>
    <h2 class="card__title">Game Development</h2>
    <p class="card__desc">
      Unity 게임 엔진을 이용해 메트로베니아 장르 게임 제작.
    </p>
  </article>

  <article class="card">
    <p class="card__tag">Research</p>
    <h2 class="card__title">ML paper</h2>
    <p class="card__desc">
      classical ML 방법론에 대한 논문 제1저자 등재.
    </p>
  </article>
</div>
```

(기존 카드에 있던 `card__foot`은 이 두 카드에 대응하는 실제 링크가 없으므로 생략한다 — 스펙 표에 명시된 필드만 채운다.)

- [ ] **Step 5: 수동 검증**

Run: `python3 -m http.server 8000` (프로젝트 루트에서)
브라우저로 `http://localhost:8000/index.html` 접속 후 확인:
- 상단 nav에 "Project" 탭이 활성(`aria-current="page"`) 상태로 표시되는가
- 히어로 문구가 새 카피로 보이는가
- 필터칩이 "전체/개발/연구" 3개로 보이는가
- 카드 그리드에 "Game Development"/"ML paper" 카드 2개만 보이는가

- [ ] **Step 6: Commit**

```bash
git add index.html
git commit -m "feat: replace starter demo content with project cards on index.html"
```

---

### Task 3: cs.html 신규 생성 — CS 탭

**Files:**
- Create: `cs.html` (about.html을 복사해 `<main>`만 교체)

**Interfaces:**
- Consumes: Task 1에서 `NAV_LINKS`에 이미 등록된 `{ href: "cs.html", label: "CS" }` — 이 파일이 존재해야 링크가 정상 동작
- Produces: 없음 (터미널 페이지)

- [ ] **Step 1: about.html을 복사해 cs.html 생성**

```bash
cp about.html cs.html
```

- [ ] **Step 2: `<head>`의 title/meta 수정**

`cs.html` 안의 기존:

```html
<title>INU Starter — About</title>
<meta name="description" content="스타터킷 소개 — 텍스트 중심 레이아웃 예시" />
```

교체:

```html
<title>INU Starter — CS</title>
<meta name="description" content="학부 재학 중 수강한 주요 과목 정리" />
```

- [ ] **Step 3: `<main>` 내용을 카드 그리드로 교체**

`cs.html`의 기존 `<main class="container-text" ...> ... </main>` 전체를 다음으로 교체:

```html
<main class="container-grid" style="padding-block: var(--space-12)">
  <p class="t-eyebrow" style="margin-bottom: var(--space-3)">CS</p>
  <h1 class="t-display-md">전공 과목</h1>
  <p class="t-body" style="margin-top: var(--space-4); margin-bottom: var(--space-8)">
    학부 재학 중 수강한 주요 전공 과목을 정리했습니다.
  </p>

  <div class="card-grid">
    <article class="card">
      <p class="card__tag">Course</p>
      <h2 class="card__title">운영체제</h2>
      <p class="card__desc">임시 텍스트</p>
    </article>

    <article class="card">
      <p class="card__tag">Course</p>
      <h2 class="card__title">데이터베이스</h2>
      <p class="card__desc">임시 텍스트</p>
    </article>

    <article class="card">
      <p class="card__tag">Course</p>
      <h2 class="card__title">디지털공학</h2>
      <p class="card__desc">임시 텍스트</p>
    </article>

    <article class="card">
      <p class="card__tag">Course</p>
      <h2 class="card__title">자료구조</h2>
      <p class="card__desc">임시 텍스트</p>
    </article>

    <article class="card">
      <p class="card__tag">Course</p>
      <h2 class="card__title">알고리즘</h2>
      <p class="card__desc">임시 텍스트</p>
    </article>
  </div>
</main>
```

- [ ] **Step 4: 수동 검증**

Run: `python3 -m http.server 8000` (이미 실행 중이 아니라면)
브라우저로 `http://localhost:8000/cs.html` 접속 후 확인:
- nav에 "CS" 탭이 활성 상태로 표시되는가
- 5개 과목 카드가 반응형 그리드로 보이는가(넓은 화면에서 3열)
- 카드 hover 시 시그널 보더 + 살짝 떠오르는 효과가 그대로 동작하는가(기존 `.card` CSS 그대로 재사용했으므로 자동으로 동작해야 함)

- [ ] **Step 5: Commit**

```bash
git add cs.html
git commit -m "feat: add CS tab page with course summary cards"
```

---

### Task 4: about.html — 자기소개로 본문 교체

**Files:**
- Modify: `about.html` (`<main>` 내부 콘텐츠 전체)

**Interfaces:**
- Consumes: 없음
- Produces: 없음 (터미널 페이지)

- [ ] **Step 1: `<main>` 내용을 자기소개로 교체**

기존 `<main class="container-text" ...> ... </main>` 전체를 다음으로 교체:

```html
<main class="container-text" style="padding-block: var(--space-12)">
  <p class="t-eyebrow" style="margin-bottom: var(--space-3)">About</p>
  <h1 class="t-display-md">이용진</h1>
  <p class="t-body" style="margin-top: var(--space-4)">
    안녕하세요, 저는 이용진입니다.
  </p>

  <h2 class="t-tagline" style="margin-top: var(--space-8)">소속</h2>
  <p class="t-body" style="margin-top: var(--space-3)">
    인천대학교 정보기술대학 컴퓨터공학부
  </p>

  <h2 class="t-tagline" style="margin-top: var(--space-8)">특기 · 취미</h2>
  <p class="t-body" style="margin-top: var(--space-3)">특기: 임의 텍스트</p>
  <p class="t-body" style="margin-top: var(--space-2)">취미: 임의 텍스트</p>

  <h2 class="t-tagline" style="margin-top: var(--space-8)">연락처</h2>
  <p class="t-body" style="margin-top: var(--space-3)">010-3221-9280</p>
</main>
```

(title/meta는 이미 "About"을 가리키고 있어 그대로 유지 — `<title>INU Starter — About</title>`)

- [ ] **Step 2: 수동 검증**

Run: `python3 -m http.server 8000` (이미 실행 중이 아니라면)
브라우저로 `http://localhost:8000/about.html` 접속 후 확인:
- "이 스타터킷에 대하여" 등 기존 문구가 전혀 남아있지 않은가
- 이름/소속/특기·취미/연락처 4개 섹션이 순서대로 보이는가
- 72ch 리딩 컬럼 레이아웃이 유지되는가

- [ ] **Step 3: Commit**

```bash
git add about.html
git commit -m "feat: replace about.html starter copy with personal bio"
```

---

### Task 5: 전체 통합 검증

**Files:**
- 없음 (검증 전용 태스크)

**Interfaces:**
- Consumes: Task 1~4의 모든 결과물
- Produces: 없음

- [ ] **Step 1: 4개 페이지 전체를 브라우저에서 순회**

Run: `python3 -m http.server 8000` (이미 실행 중이 아니라면)
브라우저로 다음을 순서대로 확인:
1. `http://localhost:8000/index.html` → nav "Project" 탭 활성, 프로젝트 카드 2개
2. `http://localhost:8000/cs.html` → nav "CS" 탭 활성, 과목 카드 5개
3. `http://localhost:8000/about.html` → nav "About" 탭 활성, 자기소개 4섹션
4. `http://localhost:8000/components.html` → 변경 없이 기존과 동일한지 확인

- [ ] **Step 2: 다크/라이트 테마 토글 확인**

각 페이지에서 우측 상단 테마 토글 버튼 클릭 → 새로 추가한 카드/텍스트 색상이 라이트 테마에서도 깨지지 않는지 확인 (기존 `var(--color-*)` 토큰만 썼으므로 자동으로 대응되어야 함)

- [ ] **Step 3: 모바일 폭에서 nav 확인**

브라우저 개발자 도구로 폭 560px 이하로 축소 → 햄버거 메뉴에 Project/CS/About/Components 4개 링크가 모두 뜨는지 확인

- [ ] **Step 4: 콘솔 에러 확인**

각 페이지에서 개발자 도구 콘솔에 에러가 없는지 확인

- [ ] **Step 5: 최종 커밋 로그 확인**

Run: `git log --oneline -6`
Expected: Task 1~4의 커밋 5개(spec 문서 포함) + 이 저장소의 이전 커밋들이 순서대로 보임
