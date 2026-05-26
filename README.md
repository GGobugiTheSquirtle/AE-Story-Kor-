# ae-story-guide

어나더에덴(Another Eden) 메인스토리 가이드. 요약/상세/전문/대사집 4가지 뷰로 스토리를 탐색할 수 있는 멀티페이지 웹 가이드.

## Tech Stack

- 순수 HTML + inline CSS + vanilla JS (빌드 도구 없음)
- Noto Serif KR 웹폰트 (동양풍 서체)
- 외부 JSON 데이터 파일 (`data/story_all.json`, 1.2MB)
- GitHub Pages + Netlify 병행 배포

## 구조

```
ae-story-guide/
├── index.html              # 허브/내비게이션 (222줄, 4개 카드)
├── summary.html            # 파트별 줄거리 요약 (541줄)
├── detail.html             # 챕터별 상세 공략 (3,729줄, ~437KB)
├── full.html               # 전문 뷰어 — JSON 로드 (578줄)
├── dialogue.html           # 대사집 뷰어 (342줄)
├── data/
│   ├── story_all.json      # 전체 스토리 데이터 (1.2MB, 118장)
│   ├── frame_mapping.json
│   ├── fullscreen_mapping.json
│   ├── truncated_dialogues.json    # gitignore (참조용)
│   ├── truncated_with_candidates.json  # gitignore
│   └── ocr/                # OCR 결과물 (gitignore)
├── fullscreen/             # 54개 비디오 폴더 (vid001~054, 1~123장 커버)
│                           # → 심볼릭 링크: /f/tmp/ae_analysis/fullscreen
├── images/
│   └── chars/              # 캐릭터 PNG 22개 (총 276종 등장 중 일부)
└── docs/
    └── archive/
        ├── 2026-03/            # 2026-03-26 라운드 아카이브
        └── 2026-05_cleanup/    # *.bak.20260325 백업 보관
```

## 동작 방식

### 페이지 구성
| 페이지 | 용도 | 데이터 소스 |
|--------|------|------------|
| `index.html` | 허브 -- 각 뷰로 링크 | 없음 (내비게이션만) |
| `summary.html` | 파트별 줄거리 요약 | HTML 인라인 |
| `detail.html` | 챕터별 상세 공략 (가장 대형) | HTML 인라인 |
| `full.html` | 전문 뷰어 | `data/story_all.json` 외부 로드 |
| `dialogue.html` | 대사집 뷰어 | `data/story_all.json` 외부 로드 |

### OCR 파이프라인
- `fullscreen/` **54개 비디오 폴더** (vid001~vid054, 각 비디오 1~3장 분량). 1~123장 전체 영상 자료 확보.
- `_tools/ocr_fullscreen.py`로 이미지에서 텍스트 추출 (EasyOCR 배치, vid001~054 전체 완료)
- `_tools/extract_dialogue_from_video.py`로 영상에서 대사 추출 (Vision API 활용)
- 추출 결과가 `data/ocr/`, `data/truncated_*.json` 등으로 저장 (모두 gitignore)

### 디자인
- 동양풍 다크 테마 (`--bg: #0b0d1a`, `--gold: #c9a44c`)
- 별빛 배경 효과 (radial-gradient)
- glassmorphism UI 요소
- 고정 topnav (48px)

## 배포

- **Netlify**: https://ae-story-kor.netlify.app/ (OG URL 기준)
- **GitHub Pages**: 독립 repo로 배포 중
- 진입점: `index.html`

## 개발 노트

- 이 프로젝트만 멀티페이지 구성 (데이터 크기로 인해 단일 파일 원칙의 예외)
- `detail.html`이 437KB로 가장 큰 파일 -- 편집 시 주의
- `.bak.20260325` 파일은 `docs/archive/2026-05_cleanup/`로 이동 (gitignore)
- 용어 통일 작업 이력: 나라다→나다라, 누알→누아르, 아크투르→아크툴, 엘자이온→엘지온, 갤리아드→갤리어드, 스지쿠→스자쿠 (커밋 로그 참조)

### Phase 진행 현황
- ✅ **Phase 1-4** (2026-03): 풀스크린 매핑 / 대사집 정비 / detail 재생성 / summary 리뉴얼
- ✅ **Phase 5** (2026-04): 짤린 대사 OCR 복원 39/40
- ✅ **Phase 6** (2026-05-26): 사실관계 정정 라운드 (부 부제 정본화, 캐릭터명 충돌 통일, index 4뷰 카드)
- ⬜ **남은 작업**:
  - ch79·109·114·119·122·123 6개 빈 챕터 OCR 데이터 보강
  - 캐릭터 초상화 보강 (현 22/276 종)
  - story_all.json 부 경계 재분류 (ch84/85 기준 공식 볼륨과 동기화)

### 부 부제 정본 (한국 공식 기준, 2026-05-26)
| 부 | 정본 |
|---|---|
| 제1부 | 시공을 넘는 고양이 |
| 제1.5부 | 오우거 전쟁편 |
| 제2부 | 동방 기담 편 · 시간의 여신의 귀환 |
| 제2.5부 | 명협계·방주편 (가이드 자체 명명) |
| 제3부 | 공허한 시층의 윤회 편 · 시간 제국의 역습 |
