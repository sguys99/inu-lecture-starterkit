# 개인 포트폴리오 커스터마이징 설계

## 배경

`inu-lecture-starterkit`는 인천대(INU) 강의용 정적 사이트 스타터킷이다. 이용진님이 이를 본인의 학부 포트폴리오로 커스터마이징한다. 요구사항은 다음 네 가지다.

1. nav의 "Home" 탭 이름을 "Project"로 변경
2. Project(구 Home) 페이지의 카드 그리드를 학기 중 활동 요약 카드로 교체
3. Home과 About 사이에 "CS" 탭을 신설해 수강 과목을 카드로 요약
4. About 탭의 본문(스타터킷 소개 ~ 다음 단계)을 자기소개로 교체

## 접근 방식

기존 페이지(index.html, about.html, components.html)는 전부 정적 HTML에 콘텐츠를 하드코딩하고, `js/layout.js`의 `NAV_LINKS` 배열로 nav/footer만 공통 관리한다. 프로젝트 원칙상 빌드 도구·데이터 기반 렌더링을 새로 들이지 않으므로, 새 페이지(CS)와 교체되는 카드도 기존과 동일하게 **정적 HTML 하드코딩**으로 작성한다. CLAUDE.md의 "새 페이지 추가" 절차(기존 페이지를 복사해 `<main>`만 교체)를 그대로 따른다.

모든 색상·간격·폰트는 `css/tokens.css` 변수만 참조하고, 카드/칩/컨테이너는 `css/components.css`에 이미 정의된 `.card`, `.chip`, `.container-grid`, `.container-text` 컴포넌트를 그대로 재사용한다. 새 CSS 클래스는 추가하지 않는다.

## 변경 사항

### 1. `js/layout.js` — 내비게이션 구조

```js
const NAV_LINKS = [
  { href: "index.html", label: "Project" },
  { href: "cs.html", label: "CS" },
  { href: "about.html", label: "About" },
  { href: "components.html", label: "Components" },
];
```

`FOOTER_COLUMNS`의 "프로젝트" 컬럼도 라벨(홈→프로젝트)과 링크(CS 추가)를 동기화한다.

### 2. `index.html` (Project 탭)

- `<title>`/`<meta description>`: "INU Starter — Project"로 변경
- 히어로: 스타터킷 안내 문구를 포트폴리오 문구로 교체 (eyebrow "Projects", 학기 중 진행한 프로젝트를 소개한다는 톤의 h1/lede). CTA 버튼(컴포넌트 보기/소개)은 그대로 둔다.
- 필터칩: "전체 / 기초 / 컴포넌트 / 배포" → **"전체 / 개발 / 연구"**. 기존에도 칩 클릭 시 실제 필터링 로직은 없었으므로(순수 시각적 `.is-active` 데모) 새 필터링 JS는 추가하지 않고 기존 동작을 그대로 유지한다.
- 카드 그리드: 기존 6개 데모 카드를 아래 2개 프로젝트 카드로 교체.

| card\_\_tag | card\_\_title | card\_\_desc |
| --- | --- | --- |
| Dev | Game Development | Unity 게임 엔진을 이용해 메트로베니아 장르 게임 제작. |
| Research | ML paper | classical ML 방법론에 대한 논문 제1저자 등재. |

### 3. `cs.html` (신규, CS 탭)

`about.html`을 복사해 `<head>`의 CSS/JS 링크와 nav/footer placeholder는 유지하고 `<main>`만 교체한다.

- `<title>`/`<meta description>`: "INU Starter — CS"
- 레이아웃: `container-grid` 안에 `card-grid`, 5개 카드. 모든 카드의 `card__tag`는 "Course"로 통일, 내용은 플레이스홀더.

| card\_\_title | card\_\_desc |
| --- | --- |
| 운영체제 | 임시 텍스트 |
| 데이터베이스 | 임시 텍스트 |
| 디지털공학 | 임시 텍스트 |
| 자료구조 | 임시 텍스트 |
| 알고리즘 | 임시 텍스트 |

### 4. `about.html` (About 탭)

"이 스타터킷에 대하여" ~ "다음 단계" 구간(h1 + 3개 h2 섹션) 전체를 아래 자기소개 콘텐츠로 교체한다. `.t-eyebrow`/`.t-display-md`/`.t-body`/`.t-tagline` 클래스와 72ch `.container-text` 리딩 컬럼 레이아웃은 그대로 유지한다.

- eyebrow: About
- h1 (`.t-display-md`): 이용진
- lead (`.t-body`): 안녕하세요, 저는 이용진입니다.
- h2 "소속": 인천대학교 정보기술대학 컴퓨터공학부
- h2 "특기 · 취미": 특기: 임의 텍스트 / 취미: 임의 텍스트
- h2 "연락처": 010-3221-9280

## 범위 밖

- 실제 필터칩 클릭 인터랙션(카드 필터링 JS)은 추가하지 않는다 — 기존 스타터킷에도 없던 기능.
- 디자인 토큰(`css/tokens.css`), 브랜드 워드마크(`inu·starter`)는 변경하지 않는다.
- `components.html`은 변경하지 않는다.

## 검증 방법

- `python3 -m http.server`로 로컬 구동 후 브라우저에서 4개 탭(Project/CS/About/Components) 모두 확인
- 각 페이지에서 nav 활성 상태(`aria-current`)가 올바른 탭에 표시되는지 확인
- 카드 그리드가 반응형 브레이크포인트(1→2→3열)에서 정상 동작하는지 확인
- 다크/라이트 테마 토글 시 새로 추가한 콘텐츠도 정상적으로 색상이 전환되는지 확인
