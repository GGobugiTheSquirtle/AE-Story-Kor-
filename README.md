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
├── index.html              # 허브/내비게이션 (210줄)
├── summary.html            # 파트별 줄거리 요약 (541줄)
├── detail.html             # 챕터별 상세 공략 (3,658줄, ~418KB)
├── full.html               # 전문 뷰어 — JSON 로드 (578줄)
├── dialogue.html           # 대사집 뷰어 (342줄)
├── data/
│   ├── story_all.json      # 전체 스토리 데이터 (1.2MB)
│   ├── frame_mapping.json
│   ├── fullscreen_mapping.json
│   ├── truncated_dialogues.json
│   ├── truncated_with_candidates.json
│   └── ocr/                # OCR 결과물 (checkpoint, batch 파일)
├── fullscreen/             # 57개 챕터 폴더 (001~057, OCR 파이프라인 소스)
├── images/
│   └── chars/              # 캐릭터 PNG 이미지
└── docs/
    └── archive/            # 아카이브된 기획 문서
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
- `fullscreen/` 57개 챕터 폴더에 게임 스크린샷 보관
- `_tools/ocr_fullscreen.py`로 이미지에서 텍스트 추출
- `_tools/extract_dialogue_from_video.py`로 영상에서 대사 추출 (Vision API 활용)
- 추출 결과가 `data/ocr/`, `data/truncated_*.json` 등으로 저장

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
- `detail.html`이 418KB로 가장 큰 파일 -- 편집 시 주의
- `.bak.20260325` 파일은 백업본 (정리 대상)
- 용어 통일 작업 이력: 나라다->나다라, 누알->누아르, 아크투르->아크툴 등 (커밋 로그 참조)
- Phase 1-2 완료, Phase 3(상세본 재생성), Phase 4(요약본 리뉴얼) 진행 중
