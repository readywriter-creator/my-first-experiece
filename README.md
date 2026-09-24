# Claude Code 플러그인 설치 가이드

Ponytail, Claude-video, Taste, Hyperframes 4개 플러그인/스킬을 Claude Code에 설치하는 절차와 사용법을 정리한 문서입니다.

## 설치 현황 (로컬 CLI 기준)

| 항목 | 버전 | 상태 | 호출 방식 |
|---|---|---|---|
| Ponytail | v4.10.0 | 설치 완료 | 세션 시작 시 자동 활성화 |
| Claude-video (watch) | v0.2.0 | 설치 완료 | `/watch [URL]`로 수동 호출 |
| Taste | 확인 필요 | 설치 완료(상세 버전·설명 확인 필요) | 문맥 인식 자동 호출 / `/design-taste-frontend` 수동 호출 |
| Hyperframes | 스킬 10개 | 설치 완료 | `/hyperframes`로 진입, 나머지는 자동 로드 |

> ⚠️ **보안 조치 필요**: `watch` 설치 시 `~/.config/watch/.env` 파일 권한이 과도하게 열려 있다는 경고가 발생했습니다. API 키 유출을 막기 위해 로컬 터미널에서 다음을 실행하십시오.
> ```
> chmod 600 ~/.config/watch/.env
> ```

## 명령어 요약

| 도구 | 명령 | 기능 |
|---|---|---|
| Ponytail | `/ponytail lite\|full\|ultra` | 강도 조절 |
| Ponytail | `/ponytail-review` | 현재 변경분에서 과설계 탐지 |
| Ponytail | `/ponytail-audit` | 저장소 전체에서 불필요한 코드 탐지 |
| Ponytail | `/ponytail-debt` | `ponytail:` 주석(미룬 작업) 모아보기 |
| Ponytail | `/ponytail-help` | 명령어 요약 |
| Ponytail | "stop ponytail" / "normal mode" | 끄기 |
| Claude-video | `/watch [URL 또는 파일] 요약해줘` | 영상 분석·요약 (yt-dlp, ffmpeg 필요; 자막 없는 영상은 Whisper API 키 필요) |
| Taste | `/design-taste-frontend` | 디자인 스타일 직접 호출 (평소엔 "랜딩 페이지 만들어줘" 등 요청 시 자동 적용) |
| Hyperframes | `/hyperframes` | 영상/모션그래픽 제작 진입점 (하위 스킬 core/animation/keyframes/audio/cli/registry/studio/creative/media-use는 필요 시 자동 로드) |

## 핵심 요약 (설치 절차 — 재설치·타 기기 설치 시 참고)

- claude.ai 채팅창에서는 로컬 컴퓨터에 직접 설치할 수 없습니다. 아래 명령을 각자의 환경(Claude Code 입력창 또는 터미널)에 직접 붙여넣어야 합니다.
- 4개 모두 합쳐 5분 내외로 설치가 끝납니다.
- Taste, Hyperframes는 Node.js가 필요합니다.

## 사전 준비

Node.js가 설치되어 있어야 합니다(Taste, Hyperframes 구동에 필요). 설치 여부가 불확실하면 Claude Code에 "Node.js 설치 여부를 확인하고 없으면 설치 방법을 알려줘"라고 요청합니다.

## 설치 순서

| 순서 | 항목 | 붙여넣을 곳 | 명령어 |
|---|---|---|---|
| 1 | Ponytail | Claude Code 입력창 | `/plugin marketplace add DietrichGebert/ponytail` 실행 후, 별도로 `/plugin install ponytail@ponytail` |
| 2 | Claude-video | Claude Code 입력창 | `/plugin marketplace add bradautomates/claude-video` 실행 후, 별도로 `/plugin install watch@claude-video` |
| 3 | Taste | 터미널 | `npx skills add https://github.com/Leonxlnx/taste-skill --skill "design-taste-frontend"` |
| 4 | Hyperframes | 터미널 | `npx hyperframes init my-video` |

**주의사항**
- 명령은 한 줄씩 따로 실행합니다. `/plugin` 명령 두 개를 한 번에 붙여넣지 않습니다.
- Taste 명령은 Claude Code에 "위 명령을 실행해 줘"라고 요청해 대신 실행시켜도 됩니다.
- Taste의 두 번째 인자(`design-taste-frontend`)는 v2(실험 버전)입니다. 문제가 생기면 `design-taste-frontend-v1`로 바꾸면 이전 버전이 설치됩니다.
- Hyperframes 명령은 프로젝트 폴더를 생성하는 단계입니다. 이후 Claude Code에 "이 폴더에서 ○○ 공지 영상을 만들어줘"라고 요청하고, 미리보기는 `npx hyperframes preview`, 영상 출력은 `npx hyperframes render`로 진행합니다. Claude Code용 Hyperframes 스킬 설치 명령은 공식 사이트(hyperframes.heygen.com)에서 확인이 필요합니다.

## 설치 확인

Claude Code를 재시작한 뒤 `/plugin`을 입력해 `ponytail`과 `watch`가 목록에 있는지 확인합니다.

- **Ponytail**: `/ponytail lite|full|ultra`로 강도를 조절하고, 끄려면 "stop ponytail"이라고 입력합니다.
- **Claude-video**: `/watch [유튜브 주소] 요약해줘`로 사용합니다. 첫 실행 시 ffmpeg와 yt-dlp 설치를 안내합니다(macOS는 자동 설치, Windows는 설치 명령 출력). Whisper API 키는 자막이 없는 영상에만 필요하며, 한국어 영상의 자막 품질은 확인이 필요합니다.

## Claude Code를 쓰지 않는 경우

Claude-video는 claude.ai 채팅창에서도 단독으로 사용할 수 있습니다.

1. 저장소 릴리스 페이지에서 `watch.skill`을 내려받습니다.
2. 설정의 Capabilities에서 "코드 실행 및 파일 생성"을 먼저 켭니다.
3. Capabilities의 Skills에서 `+` 버튼으로 파일을 올립니다.

## 보안 주의사항

- 설치 중 권한 승인을 묻는 창이 뜨면 내용을 확인한 뒤 승인합니다.
- 재정·성도 자료 등 민감한 자료가 있는 폴더에서는 외부 스킬을 실행하지 않는 것이 안전합니다. 프로젝트별 폴더를 분리해 사용합니다.

## 출처 및 근거

| 항목 | 저장소/사이트 |
|---|---|
| Ponytail | https://github.com/DietrichGebert/ponytail |
| Claude-video | https://github.com/bradautomates/claude-video |
| Taste | https://github.com/Leonxlnx/taste-skill |
| Hyperframes | https://hyperframes.heygen.com |
