# Conoti AI

**Claude Code·Codex 에 맡긴 요청을 메신저처럼 모아 보는 데스크톱 앱** — macOS · Windows

이 저장소는 Conoti AI 의 **설치 파일·릴리스 노트·업데이트 피드만** 배포합니다(소스 코드는 없습니다). 이전 이름은 AI Inbox 입니다.

여러 세션을 동시에 돌리면 먼저 끝난 작업의 결과 보고가 다른 작업에 묻히곤 합니다. Conoti AI 는 사용자 컴퓨터의 Claude Code·Codex 대화 기록을 읽어
세션을 대화방처럼, 요청 하나를 말풍선 한 쌍(내 요청 → AI 결과)으로 보여 주고, 아직 안 본 결과에만 표시를 붙여 놓치지 않게 합니다.
입력창에서 세션에 이어서 말을 보내거나 새 작업을 띄울 수 있고, 선택적으로 폰 앱 [코노티](https://conoti.app)에서도 같은 세션을 보고 답할 수 있습니다.

[English](#english)

## 설치

[Releases](https://github.com/YeoJeongHun1/conoti-ai/releases) 에서 받습니다.

| OS | 파일 |
|---|---|
| macOS (Apple Silicon · Intel) | `Conoti-AI_<버전>_universal.dmg` |
| Windows 10/11 (x64) | `Conoti-AI_<버전>_x64-setup.exe`(권장) 또는 `.msi` |

이 앱은 **Apple·Microsoft 개발자 서명이 없습니다**(macOS 는 ad-hoc 서명, 공증 없음). 그래서 처음 열 때 운영체제가 한 번 막습니다. 아래 순서대로 허용하세요.

### macOS 처음 실행 (Gatekeeper)

1. DMG 를 열고 `Conoti AI` 를 `응용 프로그램` 폴더로 끌어 옮깁니다.
2. `응용 프로그램` 에서 `Conoti AI` 를 엽니다. "열 수 없습니다"·"확인되지 않은 개발자" 창이 뜨면 **완료**(또는 **취소**)를 누릅니다.
3. **시스템 설정 → 개인정보 보호 및 보안** 아래쪽의 차단 안내 옆 **그래도 열기** 를 누르고, 다시 뜨는 창에서 **열기** 를 누릅니다(암호·Touch ID 를 물을 수 있습니다). 한 번 허용하면 다음부터는 그냥 열립니다.

### Windows 처음 실행 (SmartScreen)

1. 브라우저(Edge 등)가 "일반적으로 다운로드되지 않는 파일"이라며 막으면 내려받기 목록의 메뉴에서 **유지** 를 고르고 이어지는 확인에서도 유지를 고릅니다.
2. 설치 파일을 열었을 때 "Windows의 PC 보호" 창이 뜨면 **추가 정보** → **실행** 을 누릅니다.

처음 켜면 최근 7일(설정에서 변경)의 Claude Code·Codex 대화 기록을 읽어 채웁니다. 이때 이미 끝나 있던 요청은 읽은 것으로 들어갑니다.
훅 설치(설정 → Claude Code 훅)는 선택이지만 권장합니다 — 훅 없이도 기록은 모이고, 훅이 있으면 작업 완료·권한 대기를 즉시 받습니다.

## 업데이트

앱이 6시간마다 이 저장소의 공개 릴리스 정보(`latest.json`)만 읽어 새 버전을 알립니다(아무것도 보내지 않음 · 설정에서 끔).
**사용자가 누를 때만** 받아 설치하고, 앱에 내장된 공개키로 서명(minisign)이 맞고 버전이 일치하는 업데이트만 설치합니다.
AI Inbox 0.10.1 이하에서 올라오는 경우의 변경점(이름·폴더 등)은 릴리스 노트를 보세요.

## 받은 파일 확인

릴리스마다 `SHA256SUMS.txt` 와 업데이터 서명(`.sig`)이 함께 올라갑니다. 소스 저장소가 비공개이므로 GitHub 빌드 출처 증명(attestation)은 제공하지 않습니다.

- **SHA256SUMS** — 받은 파일과 `SHA256SUMS.txt` 를 같은 폴더에 두고:
  - macOS: `shasum -a 256 -c SHA256SUMS.txt --ignore-missing`
  - Windows PowerShell: `Get-FileHash .\<파일> -Algorithm SHA256` 의 값이 `SHA256SUMS.txt` 의 그 파일 줄과 같은지 봅니다.
- **업데이터 서명(`.sig`)** — 업데이트 묶음(`*.app.tar.gz` · `*-setup.exe`)에만 있습니다. [minisign](https://jedisct1.github.io/minisign/) 으로 직접 확인하려면(공개키는 릴리스 본문에 있습니다):
  ```sh
  base64 --decode < <파일>.sig > <파일>.minisig      # Windows: certutil -decode <파일>.sig <파일>.minisig
  minisign -Vm <파일> -x <파일>.minisig -P <릴리스 본문의 공개키>
  ```
- DMG·MSI 에는 업데이터 서명이 없습니다 — `SHA256SUMS.txt` 로 확인하세요.

## 개인정보와 데이터 위치

**원격 측정이 없고, 대화 내용을 밖으로 보내지 않습니다.** 네트워크를 쓰는 것은 둘뿐입니다: 새 버전 확인(위)과, 사용자가 켠 **폰 연결**(`conoti.app` 과 TLS).
선택 기능(대화 이력 찾기·응답 요약·태그 제안)은 기본 꺼짐이고, 켜고 동의해야만 사용자 컴퓨터에 로그인된 Claude Code/Codex CLI 로 선택된 발췌가 해당 서비스에 전송됩니다(구독 사용량 소모).

| 무엇 | 어디 |
|---|---|
| 읽기만 함 | `~/.claude/projects`·`~/.claude/sessions`(Claude Code), `~/.codex/sessions`(Codex) — 원본은 고치거나 지우지 않습니다 |
| 저장 | 앱 데이터 폴더의 `inbox.db`(요청·요약·상태) · 폰 연결 키 `relay-identity.json` · 보낸 이미지 `attachments/` |
| 데이터 폴더 | macOS `~/Library/Application Support/com.yeojeonghun.ai-inbox/` · Windows `%LOCALAPPDATA%\com.yeojeonghun.ai-inbox\` |
| 고침(사용자가 눌렀을 때만) | `~/.claude/settings.json` 의 훅 — 추가만 하고, 제거 땐 이 앱의 항목만 뺍니다 |

도구 **출력**(파일 내용·명령 결과)은 저장하지 않고 한 줄 요약만 남깁니다. 흔한 형식의 비밀값은 저장 전에 `[가림]` 으로 바꾸지만 **완벽하지 않습니다** — 내보낸 문서를 공유하기 전에 한 번 읽어 보세요.

**지우기:** 설정 → Claude Code 훅 → **제거** → 앱 삭제 → 데이터 폴더 삭제(위 표).

## 폰 연결 (선택)

1. Conoti AI 설정 → **폰 연결** → **폰 연결하기 (QR)**.
2. 코노티 앱 → 홈 → **AI 작업** → **PC 연결하기** 로 QR 을 찍습니다(찍을 수 없으면 코드 복사 → 앱의 "코드 붙여넣기").
3. PC 에 뜨는 **새 기기 연결 요청** 에서 허용합니다. 새 기기는 기본으로 **보기만** 가능하며, 답 보내기 등은 PC 에서 단계를 올려야 합니다.

폰과 PC 는 **종단간 암호화**(Noise)로 통신하고, 코노티 서버는 암호문을 넘길 뿐 읽거나 저장하지 않습니다. 서버가 보는 것은 접속 IP·시각·메시지 크기와 횟수·푸시 횟수뿐입니다.
폰 알림에는 내용이 없습니다. PC 가 꺼져 있으면 폰은 "PC 꺼져 있음"을 보여 줍니다(원본은 PC 의 `inbox.db` 뿐입니다).
폰 답은 곧 원격 명령이므로 기기별 허용·세션별 차단·전체 멈춤 등 여러 겹으로 막습니다. 폰 연결은 운영 서버(`conoti.app`)에만 연결합니다.

## 알려진 한계

- **코노티 계정으로 들어오는 기기 연결은 준비 중입니다.** 이번 판에서는 QR 로 연결한 기기만 동작하며, 계정 연결이 지원되는 판은 릴리스 노트로 알립니다.
- 코드 서명·공증이 없어 첫 실행에 운영체제 경고가 뜹니다(위 안내).
- 요약·분류·"반드시 답해야 할 것" 표시는 규칙 기반이라 틀리거나 놓칠 수 있습니다. 중요한 확인은 원래 세션에서 하세요.
- Windows 에서 Claude Code 훅(Git Bash)·로그인 자동 실행 등 일부 동작은 실기 검증이 제한적입니다. Windows 11 에서 새 설치와 0.10.1 에서 올라오는 이행(설치 폴더·바로가기·프로그램 목록·자동 시작·Claude Code 훅 경로)을 실제로 확인했습니다. 알려진 한계: 0.10.1 에서 자동 시작을 꺼 둔 사용자는 새 판을 처음 실행할 때 자동 시작이 켜질 수 있고, 앱을 제거해도 Claude Code 설정(`settings.json`)의 훅 줄이 남을 수 있어 제거 전에 앱 설정에서 훅을 먼저 제거하는 것을 권장합니다.
- Codex 는 Windows 에서 열림 여부를 알 수 없어 항상 대기열에 넣고, npm 래퍼 `codex.cmd` 는 지원하지 않습니다(`codex.exe` 필요).
- 자세한 보안 한계는 [SECURITY.md](SECURITY.md).

## 문의·지원·취약점 신고

- 문의·버그·제안: 이 저장소의 [Issues](https://github.com/YeoJeongHun1/conoti-ai/issues) (Wiki 는 쓰지 않습니다). 공개 글이므로 **비밀값·개인정보·실제 대화 내용·실제 세션이 찍힌 스크린샷을 올리지 마세요.** 신고용 버전 정보는 앱 사이드바 아래 버전 표시 → **신고용으로 복사**.
- 취약점: **공개 Issue 에 쓰지 말고** Security → **Report a vulnerability**(비공개 신고). 자세한 방법은 [SECURITY.md](SECURITY.md).

## 라이선스

- Conoti AI 0.12.0 이상: © ELD, [이용 조건](TERMS.md)을 따릅니다([LICENSE](LICENSE) 는 그 요약).
- **오픈소스 구성요소**는 각자의 라이선스를 따르며, 이름·버전·라이선스·저작권 고지는 릴리스마다 첨부된 `THIRD-PARTY-NOTICES.md` 에 있습니다.
- **0.10.1 까지**(이전 이름 AI Inbox)의 MIT 공개분은 [YeoJeongHun1/ai-inbox](https://github.com/YeoJeongHun1/ai-inbox) 에서 그대로 MIT 입니다.
- Claude·Claude Code 는 Anthropic 의, OpenAI·Codex 는 OpenAI 의 상표이며 이 앱은 두 회사와 관계가 없습니다.

## English

**Conoti AI** (formerly AI Inbox) is a desktop app (macOS · Windows) that turns your Claude Code and OpenAI Codex sessions into a messenger-style inbox: sessions are chats, each request is a pair of bubbles, and unseen results stay marked. This repository distributes **installers, release notes and the update feed only** (no source code).

- **Install:** download `Conoti-AI_<version>_universal.dmg` (macOS) or `Conoti-AI_<version>_x64-setup.exe` / `.msi` (Windows) from [Releases](https://github.com/YeoJeongHun1/conoti-ai/releases). The app is neither notarized nor Authenticode-signed. macOS: move it to Applications and open it; if blocked, System Settings → Privacy & Security → **Open Anyway**. Windows SmartScreen: **More info → Run anyway**.
- **Verify:** compare downloads with `SHA256SUMS.txt`; update bundles carry minisign signatures (`.sig`) that the app checks with a built-in public key (commands in the release notes). No GitHub build attestation is published (private source repository).
- **Privacy:** no telemetry; transcripts are read locally and a summary is stored only in a local SQLite file. The optional phone link is end-to-end encrypted (Noise) and the relay stores nothing. Updates are installed only when you click, after signature verification.
- **Known limitation:** connecting through a Conoti account is not available yet; only QR-paired devices work in this release.
- **Support:** [Issues](https://github.com/YeoJeongHun1/conoti-ai/issues). Report vulnerabilities privately (Security → Report a vulnerability), see [SECURITY.md](SECURITY.md).
- **License:** 0.12.0+ © ELD under the [Terms](TERMS.md); open-source components under their own licenses (`THIRD-PARTY-NOTICES.md`); versions through 0.10.1 remain MIT.
