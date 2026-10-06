# 보안 정책 / Security Policy

## 신고 방법 / Reporting

취약점은 **공개 Issue 에 적지 마세요.** 이 저장소의 **Security → Report a vulnerability**(GitHub 비공개 신고)로 보내 주세요.
재현 방법·영향 범위·확인한 앱 버전(앱 사이드바 아래 버전 표시 → "신고용으로 복사")을 함께 적어 주세요. 비밀값·실제 대화 내용은 적지 않아도 됩니다.

- **접수 확인 목표: 7일 이내**(일정 약속이 아니라 목표입니다). 수정 시점은 약속하지 않으며, 고친 릴리스를 낸 뒤 알립니다.
- 수정이 나올 때까지 세부를 공개하지 않도록 부탁드립니다. 원하시면 신고자 이름을 릴리스 노트에 올립니다.
- 일반 버그·사용 문의는 [Issues](https://github.com/YeoJeongHun1/conoti-ai/issues) 로 보내 주세요.

Please report vulnerabilities **privately** via **Security → Report a vulnerability** on this repository — never as a public issue.
Include steps to reproduce, impact, and the app version. Vulnerabilities in the Conoti phone app or server are out of scope here; please use https://conoti.app/support. We aim to acknowledge reports within 7 days (a target, not a guarantee), and will disclose after a fixed release is out.

## 지원 버전 / Supported versions

**가장 최근 릴리스만** 보안 수정을 받습니다. / Only the latest release receives security fixes.
(0.10.1 이하 — 이전 이름 AI Inbox — 는 지원하지 않습니다.)

## 범위 / Scope

- **범위 안:** Conoti AI 데스크톱 앱(설치 파일에 들어 있는 것), 앱 안 업데이트 사슬(`latest.json`·서명 검증·릴리스 자산), PC 와 폰 사이 연결 규약의 PC 쪽 동작.
- **범위 밖:** 이미 사용자 계정 권한을 가진 로컬 공격자 · 사용자가 직접 공유한 내보내기 파일의 내용 · 폰 앱(코노티)과 코노티 서버 자체의 취약점(이 창구로 받지 않습니다 — [코노티 지원·문의](https://conoti.app/support)로 알려 주세요) · Claude Code·Codex·GitHub 자체의 취약점.

## 설계상 경계 / Security model (요약)

아래를 깨는 동작은 취약점으로 봅니다.

- **대화 내용은 밖으로 나가지 않습니다.** 원격 측정이 없고, 앱이 부르는 원격 주소는 새 버전 확인용 `github.com` 과, 사용자가 켠 폰 연결용 `conoti.app` 뿐입니다(사용자가 켜고 동의한 선택 기능은 사용자 컴퓨터의 Claude Code/Codex CLI 를 거쳐 그 서비스로 갑니다). 운영 빌드는 다른 서버를 고르는 설정이 없습니다.
- **업데이트는 서명된 것만.** 앱에 고정된 공개키(minisign)로 서명을 확인하고 버전이 일치하는 것만 설치하며, 사용자가 누를 때만 받습니다. 서명 개인키는 소스·릴리스 밖의 비공개 위치에만 있습니다.
- **중계 서버를 믿지 않습니다.** 폰과 오가는 내용은 종단간 암호문(Noise `IKpsk2_25519_ChaChaPoly_SHA256`)이고 서버는 짝지어 넘길 뿐입니다. 키 교환은 폰이 PC 화면의 QR 을 직접 읽어 하며, 연결 전에 PC 에서 사용자가 허용해야 합니다. 서버·중간자는 내용을 읽거나 폰 답을 만들 수 없습니다. 푸시에는 내용이 없습니다.
- **기기 권한은 PC 가 정합니다.** 새 기기는 기본 "보기만"이고, 권한을 **올리는 것은 PC 화면에서만** 됩니다. 폰이 부를 수 있는 것은 정해진 목록(세션 목록·대화·문서·읽음·답·이미지·세션 관리·예약 등)뿐이며 파일 읽기·명령 실행 통로는 없습니다.
- **폰 답은 원격 명령으로 취급해 여러 겹으로 막습니다.** 허용한 기기의 암호 통로로 온 답만 · 기기별 허용 · 세션별 차단 · 전체 멈춤 · 선택적 데스크톱 확인 · 길이·제어문자 검사 · 전달 기록. 권한 옵션을 넓혀 실행하지 않습니다.
- **실행은 셸 없이 인자로**(`claude`·`codex`), 사용자가 보냈거나 시작했을 때만. 훅은 `~/.claude/settings.json` 에 추가만 하고 제거 때는 이 앱의 항목만 뺍니다.
- **화면 격리.** 엄격한 CSP, 외부 주소 탐색 차단, 대화 기록 속 HTML 은 렌더링하지 않음, 원격 이미지 불러오지 않음.
- **데이터 보호.** 데이터 폴더 `700`·DB `600`(macOS/Linux). 도구 출력은 저장하지 않고, 흔한 비밀값 형식은 저장 전에 가립니다(최선 노력).

## 알려진 한계 / Known limitations

- **비밀값 가림은 최선 노력입니다.** 흔한 형식만 잡으며 새로운 꼴(가리키는 낱말 없이 적은 값 등)은 놓칠 수 있고, **세션 화면에서는 원문이 그대로 보입니다.** "반드시 답해야 할 것"('!' 표시) 목록에서도 코드·비밀값을 가리지만 같은 한계가 있습니다. 내보낸 문서를 공유하기 전에 직접 확인하세요.
- **폰 연결 키가 파일에 저장됩니다.** PC 의 비밀키(`relay-identity.json`)는 키체인이 아니라 앱 데이터 폴더의 파일(macOS/Linux `600`)에 있어 DB 와 같은 보호 수준입니다(서명 없는 앱은 업데이트마다 키체인 허용 창이 떠서 키체인을 쓰지 않습니다). Windows 는 별도 모드 비트 없이 사용자 프로필 권한을 따릅니다.
- **Windows 한계.** Claude Code 훅(Git Bash)·로그인 자동 실행은 실기 검증이 제한적입니다. 단일 실행 판정은 같은 로그인 세션의 다른 프로그램이 같은 이름을 먼저 만들면 풀릴 수 있습니다. Windows 11 에서 새 설치와 0.10.1 에서 올라오는 이행(설치 폴더·바로가기·프로그램 목록·자동 시작·Claude Code 훅 경로)을 실제로 확인했습니다. 알려진 한계: 0.10.1 에서 자동 시작을 꺼 둔 사용자는 새 판을 처음 실행할 때 자동 시작이 켜질 수 있고, 앱을 제거해도 Claude Code 설정(`settings.json`)의 훅 줄이 남을 수 있어 제거 전에 앱 설정에서 훅을 먼저 제거하는 것을 권장합니다.
- **코드 서명·공증이 없습니다.** 받은 파일은 릴리스의 `SHA256SUMS.txt` 와 비교하고, 업데이트 묶음은 `.sig` 로 확인하세요(방법은 README). 소스가 비공개라 GitHub 빌드 출처 증명은 제공하지 않습니다.
- **채널**은 Claude Code 의 연구 미리보기 기능이며 허용 목록 밖이라 `--dangerously-load-development-channels` 로 켜야 합니다.
- **폰 연결을 켜면** 중계 서버는 메타데이터(접속 IP·시각·메시지 크기와 횟수·푸시 횟수)를 볼 수 있습니다. 내용은 볼 수 없습니다.
- 폰(코노티 앱) 자체는 이 저장소 밖입니다. 복호화된 내용은 그 앱 화면에만 있습니다.
- **코노티 계정으로 들어오는 기기 연결은 이 판에서 아직 열려 있지 않습니다**(QR 로 연결한 기기만 동작).
