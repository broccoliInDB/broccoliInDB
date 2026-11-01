# ccoli (콜리)

> **누가 작업해도 일관성이 있게 고도화 되도록 설계하는 걸 지향함.**
> 따라서 룰과 훅, 린트로 시스템을 설계해서 dogfood로 기대와 얼마나 다른지 체크하고, 반복함으로 기대 방향으로 구현하며, 이런 시스템을 자동화하려는 지향점을 갖고 있음.

지향하는 구조의 예.

### 구조 (1) — RN 베이스

```
RN 모노레포 (pnpm + Turborepo)
├ apps/
│  ├ rn ············· 앱 본체
│  ├ preview ········ OTA/빌드 미리보기
│  ├ storybook ······ 컴포넌트 카탈로그
│  ├ web / be ······· 웹 · 백엔드
│  └ screenshot-agent  캡처 자동화 에이전트
└ packages/
   ├ design-system/
   │  ├ components ·· 화면 조립 단위 (Button, Card, BottomSheet …)
   │  ├ tokens ······ 색·간격 같은 원천값
   │  └ utils ······· 공용 유틸
   ├ feature-*  ····· 도메인별 기능 패키지
   └ harness ········ 룰·린트·게이트 (구조 2)
```

```
apps/rn/
├ core ······ 공통 커널. 네비·가격·토큰을 가공된 래퍼로만 노출
│             → 하위에서 원시값 직접 접근이 불가능하게 막음
├ shared ···· 도메인 공용 요소
├ domains ··· 도메인별 경계 (서로 직접 참조 안 함)
├ composite · 도메인을 엮는 화면
├ shell ····· 앱 셸 (진입·레이아웃·네비 호스트)
├ libs ······ 외부 라이브러리 어댑터
├ modules ··· 네이티브/플랫폼 모듈
└ plugins ··· 빌드 플러그인
```

### 구조 (2) — 하네스

```
harness (packages/harness + scripts + hooks + agent)
├ rules/ ········· 설계 원칙을 린트 규칙으로 박음 (60여 개, 각각 .test)
│                  noAdHoc*  손으로 조립 금지 (DS 컴포넌트 두고)
│                  noRaw*    원시값 직접 쓰기 금지 (래퍼 두고)
│                  require*  빠지면 안 되는 것 강제
├ scripts/ ······· 린트로 안 되는 검사
│                  import 사이클 차단 · 구조 체크 · 결정번호/주석좌표 체크
├ baselines/ ····· *-gate.json = 현재 쌓인 빚의 기준치
│                  (dogfood로 재측정, 갚을수록 기준치가 내려감)
├ gate.sh ········ 위를 한 번에 돌리는 관문. 병렬 슬롯으로 스케줄
│                  멤버는 소속 fragment·공통 task(format/type-check/test)·
│                  앵커 설정을 다 갖춰야 통과. 하나 빠지면 초록 안 내고 멈춤
├ hooks/ ········· 커밋·편집 경계에서 선차단 (fail-closed)
│                  scan-secret            시크릿 유출 차단
│                  block-raw-merge        raw merge 차단
│                  block-unloaded-edit    규칙 안 실린 채 편집·커밋 차단
│                  block-asset-write      금지 경로 쓰기 차단
│                  ai-precommit-autofix   AI가 커밋 전 자동 수정
└ agent/ax-worker  격리 worktree의 AI 워커
                   설계→구현→셀프리뷰→게이트→PR, 막히면 추측 대신 질문
```

### 성향

- 예방지향
- 도구 고집없음
- 휘발적인 업무방식 지양
- 설계 문서지향형이지만 융통성 있음
- 주도적인 업무태도

### 기술

`TypeScript` · `React Native / Expo` · `React Query` · `pnpm + Turborepo 모노레포(DDD)` · `EAS(OTA)` · `커스텀 ESLint + vitest` · `GitHub Actions` · `Firebase / FCM / Crashlytics`

### 더 보기

→ **https://broccoliindb.github.io**

---

<details>
<summary><b>🇬🇧 English</b></summary>

<br>

> **I design so that anyone's work stays consistent and keeps getting better.**
> So I build the system out of rules, hooks and lint, use dogfooding to check how far it is from intent, and iterate toward intent — and I aim to automate that system itself.

An example of the structure I aim for.

### Structure (1) — the RN base

```
RN monorepo (pnpm + Turborepo)
├ apps/
│  ├ rn ············· the app itself
│  ├ preview ········ OTA/build preview
│  ├ storybook ······ component catalog
│  ├ web / be ······· web · backend
│  └ screenshot-agent  capture-automation agent
└ packages/
   ├ design-system/
   │  ├ components ·· building blocks (Button, Card, BottomSheet …)
   │  ├ tokens ······ source values (color, spacing)
   │  └ utils ······· shared utils
   ├ feature-*  ····· per-domain feature packages
   └ harness ········ rules · lint · gate (Structure 2)
```

```
apps/rn/
├ core ······ shared kernel. navigation, price, tokens exposed only via
│             governed wrappers → raw primitives can't be reached below
├ shared ···· cross-domain shared pieces
├ domains ··· domain boundaries (no direct references between them)
├ composite · screens that wire domains together
├ shell ····· app shell (entry, layout, nav host)
├ libs ······ adapters for third-party libs
├ modules ··· native/platform modules
└ plugins ··· build plugins
```

### Structure (2) — the harness

```
harness (packages/harness + scripts + hooks + agent)
├ rules/ ········· design principles frozen as lint rules (~60, each .test)
│                  noAdHoc*  no hand-assembly (a DS component exists)
│                  noRaw*    no raw primitive (a wrapper exists)
│                  require*  enforce what must not be missing
├ scripts/ ······· checks lint can't express
│                  import-cycle block · structure check · decision/comment checks
├ baselines/ ····· *-gate.json = the current baseline of accrued debt
│                  (re-measured by dogfooding; it drops as debt is paid down)
├ gate.sh ········ runs all of the above at once, scheduled over parallel slots
│                  a member passes only with its fragment + shared tasks
│                  (format/type-check/test) + anchor config; miss one, it halts
├ hooks/ ········· blocked at the commit/edit boundary (fail-closed)
│                  scan-secret            block secret leaks
│                  block-raw-merge        block raw merges
│                  block-unloaded-edit    block edits/commits without rules loaded
│                  block-asset-write      block writes to forbidden paths
│                  ai-precommit-autofix   AI fixes before commit
└ agent/ax-worker  an AI worker in an isolated worktree
                   design→build→self-review→gate→PR; asks instead of guessing
```

### Style

- Prevention-first
- No attachment to tools
- Avoid ephemeral, ad-hoc ways of working
- Design/doc-oriented, but flexible
- Proactive by default

### Tech

`TypeScript` · `React Native / Expo` · `React Query` · `pnpm + Turborepo monorepo (DDD)` · `EAS (OTA)` · `custom ESLint + vitest` · `GitHub Actions` · `Firebase / FCM / Crashlytics`

### More

→ **https://broccoliindb.github.io**

</details>
