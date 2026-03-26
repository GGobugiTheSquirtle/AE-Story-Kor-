# PDCA Completion Report: ae-story-guide

> 어나더에덴 메인스토리 가이드 — GitHub Pages 배포 완료 보고서

---

## 1. Overview

| 항목 | 내용 |
|------|------|
| Feature | 어나더에덴(Another Eden) 메인스토리 전체 122장 가이드 |
| Repository | [AE-Story-Kor-](https://github.com/GGobugiTheSquirtle/AE-Story-Kor-) |
| Frame Images Repo | [AE-Story-Frames](https://github.com/GGobugiTheSquirtle/AE-Story-Frames) |
| 배포 URL | https://ggobugithesquirtle.github.io/AE-Story-Kor-/ |
| 작업 기간 | 2026-03-23 (09:10 ~ 11:51 KST) |
| 총 커밋 | 5 commits |
| 총 코드량 | 1,826 lines (3 HTML files) |

---

## 2. Deliverables

### 2.1 페이지 구성 (3 pages)

| 파일 | 라인 | 역할 |
|------|------|------|
| `index.html` | 370 | 랜딩 페이지 — 제1부~제3부 챕터 네비게이션 |
| `detail.html` | 879 | 상세 스토리 뷰어 — 씬 카드, 대사 블록쿼트, 핵심 포인트 |
| `full.html` | 577 | 풀버전 채팅 뷰어 — 전체 대사 + 씬 프레임 이미지 인라인 |

### 2.2 데이터

| 파일 | 크기 | 내용 |
|------|------|------|
| `data/story_all.json` | 1,077 KB | 전 122장 대사 데이터 |
| `data/frame_mapping.json` | 17 KB | 119개 챕터→프레임 매핑 (vid, frame_start, frame_end) |

### 2.3 캐릭터 초상화

`images/chars/` — 14명 플레이어블 캐릭터 아이콘 (PNG)
- 알도, 피네, 사이러스, 에이미, 리이카, 헬레나, 길드너, 알테나
- 애쉬티어, 캄라녜쥬, 베레토, 미나르카, 질퍼니, 니르야
- NPC는 CSS 이니셜 폴백 (색상 해시 기반)

### 2.4 씬 프레임 이미지 (별도 Repo)

| 항목 | 수치 |
|------|------|
| 총 프레임 수 | 13,842장 |
| 원본 크기 | 7.5 GB (PNG sprite sheets) |
| 최종 크기 | 144 MB (800px WebP, quality 75) |
| 비디오 수 | 54개 |
| CDN URL | https://ggobugithesquirtle.github.io/AE-Story-Frames/ |

---

## 3. Commit History

| # | Hash | 시간 | 내용 |
|---|------|------|------|
| 1 | `6643da4` | 09:10 | 초기 배포 — 전 122장 완전판 (제1부~제3부) |
| 2 | `e3919f0` | 09:23 | 주요 캐릭터 초상화 아이콘 적용 (14명) |
| 3 | `3e148d0` | 09:58 | 씬 프레임 이미지 삽입 (13,842장, lazy loading) |
| 4 | `5b888b2` | 10:05 | 어나더에덴 게임 UI 테마 전면 리디자인 |
| 5 | `ad5db48` | 11:51 | 검색 하이라이트 DOM 버그 + 프레임 인덱스 수정 |

---

## 4. Design — 어나더에덴 게임 UI 테마

### 4.1 컬러 팔레트

| CSS Variable | 값 | 용도 |
|-------------|------|------|
| `--bg-deep` | `#0b0d1a` | 메인 배경 (심연 인디고) |
| `--bg-panel` | `#141627` | 패널/카드 배경 |
| `--gold` | `#c9a44c` | 주요 악센트, 테두리, 타이틀 |
| `--gold-dim` | `#8a7235` | 보조 골드 (구분선, 서브텍스트) |
| `--text` | `#d8d4cc` | 본문 텍스트 |

### 4.2 디자인 요소

- **서체**: Noto Serif KR (세리프) — 판타지 RPG 분위기
- **다이아몬드 장식**: 이름판 양쪽 `::before`/`::after` 다이아몬드 pseudo-elements
- **별빛 배경**: CSS `radial-gradient` 기반 starfield 파티클
- **골드 테두리**: 대화 버블, 카드, 패널에 `border: 1px solid var(--gold-dim)`
- **장식 구분선**: `◆ ─── ◆` 패턴 ornate dividers

### 4.3 UI 통일성

3개 페이지 모두 동일한 CSS 변수 + 디자인 언어 적용:
- topnav 높이 48px, 동일 골드 하단 보더
- 카드/패널 동일한 `--bg-panel` + 골드 보더 패턴
- 반응형 모바일 대응 완료

---

## 5. Technical Implementation

### 5.1 성능 최적화

| 기술 | 적용 대상 | 효과 |
|------|----------|------|
| IntersectionObserver | 챕터 렌더링 | 뷰포트 진입 시에만 DOM 생성 |
| `loading="lazy"` | 13,842 프레임 이미지 | 네이티브 브라우저 레이지 로딩 |
| WebP (quality 75) | 프레임 이미지 | 7.5GB → 144MB (98% 압축) |
| 별도 CDN Repo | 프레임 이미지 | 메인 repo 경량화 |

### 5.2 프레임 매핑 알고리즘

- 54개 비디오 → 122개 챕터 매핑
- 각 비디오 내 대사 줄 수 기반 비례 프레임 배분
- `frame_mapping.json` 119개 엔트리 (프레임 없는 챕터 제외)

### 5.3 검색 기능

- 전체 렌더 강제 후 텍스트 검색
- **텍스트 노드 전용 하이라이트**: `hlText()` 재귀 함수로 DOM 안전 처리
- **안전한 하이라이트 제거**: `<mark>` 자식 노드를 부모에 이동 후 제거 + `normalize()`
- Enter 키로 다음 결과 순회, 건수 표시

---

## 6. Bug Fixes (Review → Fix)

### 리뷰에서 발견된 HIGH 버그 3건 → 전수 수정 완료

| # | 버그 | 원인 | 수정 |
|---|------|------|------|
| 1 | 프레임 인덱스 중복/누락 | `Math.round(i/frameStep)` 비선형 계산 | `nextFrame` 순차 증가로 변경 |
| 2 | 검색 하이라이트 제거 시 DOM 파괴 | `el.outerHTML=el.textContent`가 `.name` span 등 삭제 | DOM 노드 이동 방식으로 `<mark>`만 제거 |
| 3 | 검색 하이라이트 삽입 시 HTML 속성 오염 | `innerHTML.replace(re,...)` 가 태그 속성도 매칭 | 텍스트 노드(nodeType===3)만 재귀 탐색 |

---

## 7. Quality Assessment

| 항목 | 상태 | 비고 |
|------|------|------|
| 테마 일관성 (3페이지) | ✅ PASS | 동일 CSS 변수, 디자인 언어 |
| 데이터 무결성 | ✅ PASS | 122장 전체 로드 확인 |
| 프레임 매핑 정합성 | ✅ PASS | 119 엔트리, off-by-1 경고 수준 |
| 검색 기능 | ✅ PASS | DOM 안전 하이라이트 (3건 버그 수정 완료) |
| 캐릭터 아이콘 | ✅ PASS | 14명 매핑 + NPC 폴백 |
| 모바일 반응형 | ✅ PASS | 3페이지 모두 대응 |
| GitHub Pages 배포 | ✅ PASS | 메인 repo + 프레임 CDN repo |

---

## 8. Architecture Diagram

```
AE-Story-Kor- (GitHub Pages)
├── index.html          ← 랜딩 (챕터 네비게이션)
├── detail.html         ← 상세 뷰어 (씬 카드)
├── full.html           ← 풀 채팅 뷰어 + 프레임 이미지
├── data/
│   ├── story_all.json  ← 122장 대사 데이터
│   └── frame_mapping.json ← 챕터→프레임 매핑
└── images/chars/       ← 14명 캐릭터 아이콘

AE-Story-Frames (GitHub Pages CDN)
└── {001~054}/          ← 54 비디오 폴더
    └── {0001~NNNN}.webp ← 13,842 프레임 이미지
```

---

## 9. Lessons Learned

1. **이미지 압축 전략**: PNG sprite sheets 7.5GB → WebP 800px quality 75 → 144MB. GitHub repo 1개에 수용 가능한 수준으로 98% 압축 달성
2. **DOM 안전 검색**: `innerHTML.replace()`는 HTML 속성까지 매칭하므로 텍스트 노드 전용 재귀 탐색 필수
3. **게임 UI 재현**: CSS 변수 + pseudo-elements로 어나더에덴 특유의 골드+인디고 테마를 순수 HTML/CSS로 충실히 재현 가능

---

*Generated: 2026-03-23 | ae-story-guide PDCA Completion Report*
