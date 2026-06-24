# DESIGN.md — 허리디스크 회복연구소

> AI 에이전트에게: UI 작업 전 반드시 이 파일을 먼저 읽고 아래 시스템을 따른다.
> 임의로 색상·폰트·간격·컴포넌트 패턴을 변경하지 않는다.

---

## 1. 브랜드 정체성

**핵심 감성:** 과학적 신뢰 + 인간적 회복의 온기
**포지셔닝:** 병원이 아닌 동반자. 데이터가 있는 코칭.
**피해야 할 느낌:** 차갑고 기계적인 의료, 과장된 광고, 일반적인 헬스 사이트

---

## 2. 컬러 팔레트

```css
:root {
  /* 브랜드 그린 — 회복·성장·자연 */
  --g900: #064e3b;   /* 메인 다크그린: 네비바, 주요 배경 */
  --g700: #047857;   /* 중간 그린: 호버, 보조 강조 */
  --g500: #10b981;   /* 에메랄드: 포인트 컬러 */
  --g400: #34d399;   /* 밝은 에메랄드: 텍스트 강조, 아이콘 */
  --g100: #d1fae5;   /* 연한 민트: 배지, 태그 배경 */
  --g50:  #ecfdf5;   /* 아주 연한 민트: 카드 배경 */

  /* 뉴트럴 */
  --gray9: #111827;  /* 본문 텍스트 */
  --gray7: #374151;  /* 보조 텍스트 */
  --gray5: #6b7280;  /* 캡션, 플레이스홀더 */
  --gray2: #e5e7eb;  /* 디바이더, 보더 */
  --gray1: #f9fafb;  /* 섹션 배경 */

  /* 페이지 배경 */
  --bg: #f8fffe;     /* 살짝 민트가 도는 화이트 */
}
```

**사용 원칙:**
- g900은 네비바·CTA 배경에만 사용. 남발 금지.
- g400은 텍스트 위 강조·아이콘 컬러로만 사용.
- 흰 배경 대신 --bg 또는 --g50 사용.

---

## 3. 타이포그래피

**폰트:** Pretendard (한국어 최적화, 이미 CDN 로드됨)

```
디스플레이 (히어로 제목):  700~800 weight, clamp(1.8rem, 4vw, 3rem)
섹션 제목 (h2):           700 weight, clamp(1.3rem, 2.5vw, 1.8rem)
카드 제목 (h3):           600 weight, 1.1rem~1.2rem
본문:                     400 weight, 1rem, line-height 1.7
캡션/레이블:              500 weight, 0.82rem, letter-spacing 0.04em
강조 레이블 (ALL CAPS):   800 weight, 0.9rem, letter-spacing 2px
```

**원칙:**
- 한국어 본문은 line-height 1.7 이상 유지
- 제목은 반드시 clamp()로 반응형 처리
- 영문 레이블(UPPERCASE)은 letter-spacing: 2px

---

## 4. 간격 시스템 (8px 기준)

```
xs:  0.5rem  (8px)   — 아이콘↔텍스트 간격
sm:  1rem    (16px)  — 인라인 요소 간격
md:  1.5rem  (24px)  — 카드 내부 패딩
lg:  2.5rem  (40px)  — 섹션 내 요소 간격
xl:  4rem    (64px)  — 섹션 상하 패딩
2xl: 6rem    (96px)  — 히어로 상하 패딩
```

---

## 5. 컴포넌트 패턴

### 카드
```css
border-radius: 16px;
background: white;
box-shadow: 0 2px 12px rgba(6, 78, 59, 0.08);
padding: 1.5rem;
border: 1px solid var(--gray2);
transition: transform 0.2s, box-shadow 0.2s;

/* 호버 */
transform: translateY(-3px);
box-shadow: 0 8px 24px rgba(6, 78, 59, 0.14);
```

### CTA 버튼 (주)
```css
background: var(--g900);
color: white;
padding: 0.9rem 2rem;
border-radius: 10px;
font-weight: 700;
font-size: 1rem;
border: none;
cursor: pointer;
transition: background 0.2s, transform 0.15s;

/* 호버 */
background: var(--g700);
transform: translateY(-1px);
```

### CTA 버튼 (보조)
```css
background: transparent;
color: var(--g900);
border: 2px solid var(--g900);
/* 나머지 동일 */
```

### 배지/태그
```css
background: var(--g100);
color: var(--g900);
padding: 0.3rem 0.8rem;
border-radius: 999px;
font-size: 0.82rem;
font-weight: 600;
```

### 섹션 구분선 대신 — 배경색 교체 방식 사용
```
white → --g50 → white → --gray1 순서로 섹션 배경 교체
hr 사용 금지
```

---

## 6. 레이아웃 원칙

- **최대 너비:** 1100px, 좌우 auto margin
- **모바일 패딩:** 좌우 1.2rem
- **그리드:** CSS Grid 사용, 복잡한 레이아웃엔 grid-template-columns
- **카드 그리드:** `repeat(auto-fit, minmax(280px, 1fr))`
- **히어로 최소 높이:** 280px 이상

---

## 7. 모션 & 애니메이션

```css
/* 표준 전환 */
transition: all 0.2s ease;

/* 페이드인 (스크롤 진입 시) */
opacity: 0 → 1, transform: translateY(16px) → 0
duration: 0.5s, ease-out

/* 무거운 애니메이션 금지 */
/* 단순 hover 효과만 허용, JS 의존 모션 최소화 */
```

---

## 8. 신뢰 신호 (Trust Signals) 패턴

데이터와 숫자는 항상 강조 표시:
```html
<span class="stat-number">XX명</span>
<span class="stat-label">회복 완료</span>
```

```css
.stat-number {
  font-size: clamp(2rem, 4vw, 3rem);
  font-weight: 800;
  color: var(--g900);
  display: block;
}
.stat-label {
  font-size: 0.9rem;
  color: var(--gray5);
}
```

---

## 9. 반응형 브레이크포인트

```css
/* 모바일 우선 */
/* 태블릿 */
@media (min-width: 640px) { ... }
/* 데스크탑 */
@media (min-width: 1024px) { ... }
```

---

## 10. 금지 사항

- ❌ Bootstrap, Tailwind CDN 추가 금지 (기존 순수 CSS 유지)
- ❌ 보라색, 파란색 계열 컬러 사용 금지
- ❌ Inter 폰트 금지 (Pretendard만 사용)
- ❌ hr 태그로 섹션 구분 금지
- ❌ 3개 이상 그림자 중첩 금지
- ❌ 의료 이미지(수술, X-ray 등) 분위기 금지 — 회복과 일상 중심

---

## 서브페이지 목록

| 파일 | 목적 | 접근 |
|---|---|---|
| index.html | 메인 허브 | 전체 공개 |
| 회복사례.html | 참여자 스토리 + VAS 데이터 | 전체 공개 |
| 데이터.html | 회복 데이터 시각화 | 전체 공개 |
| 신청하기.html | 프로그램 신청 | 전체 공개 |
| 가이드북.html | 회원 전용 가이드북 | 인증 필요 |
