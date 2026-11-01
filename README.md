# ccoli (콜리)

> **I build the structure that keeps things on-track when I'm not in the room.**
> _Design decisions frozen into lint rules; repetitive human work turned into hands-off automation._

App engineer since 2015. I started out obsessed with what every line actually *means* — which import, which package, and why — and I still am. These days that same instinct goes into **structure**: I'd rather prevent a problem than fire-fight it, so I build the gates and automation that make the right thing happen by default.

### How I work

- **Prevention over firefighting.** I predict failure modes from experience and close them *before* they ship. Fail-closed by reflex — when in doubt, block, don't silently pass.
- **Design → implement → dogfood, on repeat.** I'd rather get a stable, efficient direction right and refine it than ship something clever once.
- **Not dogmatic.** Lately I sketch the design with AI first, argue out the better approach, then build. The structure is the point — not any particular tool.
- **Consensus by construction.** In a team I keep meetings few and agenda'd, drive them to an actual decision, keep schedules realistic, and when people pull in different directions I push hard for a workable compromise.

### Two things I've built

> Structure and intent only — the source is private; no internal code or names here.

**1 — Design decisions you can't break.**
~110 custom lint rules, each a frozen design principle with its own test: *"don't hand-assemble what a design-system component already owns," "a failure is announced exactly once," "no raw primitive where a governed wrapper exists."* The payoff: a large app with many contributors — and now AI agents — stays on one design direction without me reviewing every line.

```
 base ───────────┐
                 ├─▶ variants sync from the base
 guardrail ──────┘   (protection rules stop the base from drifting)

 design ─▶ implement ─▶ dogfood ─┐
    ▲                            │
    └──────────── repeat ────────┘
```

**2 — Repetitive human work, automated away.**
A scheduled pipeline that reads the release schedule, drives the app in a simulator to capture each screen, guards disk usage and retries, then posts the results straight to the team's channel — turning a manual, daily QA chore for a non-dev team into a hands-off job.

### Tech

`TypeScript` · `React Native / Expo` · `React Query` · `pnpm + Turborepo monorepo (DDD)` · `EAS Build / Update (OTA)` · `custom ESLint rules + vitest` · `GitHub Actions` · `Firebase / FCM / Crashlytics`

### See it in motion

I rebuilt my intro as a scrollable experience — a cinematic landing, an IDE-style résumé… and something you can walk into, if you're curious.

**→ https://broccoliindb.github.io**

---

<details>
<summary><b>🇰🇷 한국어</b></summary>

<br>

> **내가 그 자리에 없어도 일이 방향대로 굴러가게 하는 '구조'를 만듭니다.**
> _설계 결정은 어길 수 없는 린트 규칙으로, 사람의 반복 노동은 무인 자동화로._

2015년부터 앱을 만들어 왔습니다. 처음엔 한 줄 한 줄이 *무슨 의미*인지 — 어떤 import, 어떤 패키지를, 왜 쓰는지 — 에 집착했고, 지금도 그렇습니다. 요즘은 그 본능이 **구조**로 향합니다. 터진 걸 수습하기보다 **예방**하는 쪽이라, 올바른 결과가 기본값이 되도록 게이트와 자동화를 만듭니다.

#### 일하는 방식

- **수습보다 예방.** 경험으로 실패 지점을 미리 예측해 *배포 전에* 막습니다. 의심되면 조용히 통과가 아니라 막는 fail-closed가 반사신경입니다.
- **설계 → 구현 → 개밥주기, 반복.** 한 번의 영리함보다, 안정적이고 효율적인 방향을 제대로 잡아 다듬는 쪽을 택합니다.
- **고집부리지 않습니다.** 요즘은 AI와 먼저 설계를 그려보고 더 나은 방식을 토론한 뒤 구현합니다. 핵심은 구조지 특정 도구가 아닙니다.
- **합의를 구조로.** 팀에선 꼭 필요한 회의만 아젠다와 함께 잡고 결론까지 끌고 가며, 일정을 현실적으로 조율하고, 의견이 갈릴 땐 실행 가능한 타협안을 끝까지 만듭니다.

#### 내가 만든 두 가지

> 구조와 의도만 공개합니다 — 소스는 비공개이며 내부 코드·명칭은 담지 않았습니다.

**1 — 어길 수 없는 설계 결정.**
커스텀 린트 규칙 약 110개. 각각이 하나의 설계 원칙이고 자체 테스트가 붙어 있습니다. *"디자인 시스템 컴포넌트가 있는데 손으로 조립하지 말 것", "실패 안내는 정확히 한 번", "가공된 래퍼가 있으면 원시값을 직접 쓰지 말 것."* 결과적으로 큰 앱에 여러 명 — 이제는 AI 에이전트까지 — 이 붙어도, 제가 모든 라인을 보지 않아도 한 방향을 유지합니다.

```
 base ───────────┐
                 ├─▶ 파생들이 base에서 sync
 guardrail ──────┘   (보호 규칙이 base의 drift를 차단)

 설계 ─▶ 구현 ─▶ 개밥주기 ─┐
    ▲                      │
    └────────── 반복 ──────┘
```

**2 — 사람의 반복 노동을 자동화로 제거.**
편성 스케줄을 읽어 시뮬레이터에서 앱을 몰고 다니며 화면을 캡처하고, 디스크·재시도를 가드한 뒤, 결과를 팀 채널에 바로 올리는 스케줄 파이프라인. 비개발 직군이 매일 수동으로 하던 QA 반복 작업을 무인 잡으로 바꿨습니다.

#### 기술

`TypeScript` · `React Native / Expo` · `React Query` · `pnpm + Turborepo 모노레포 (DDD)` · `EAS Build / Update (OTA)` · `커스텀 ESLint 규칙 + vitest` · `GitHub Actions` · `Firebase / FCM / Crashlytics`

#### 직접 보기

제 소개를 스크롤 경험으로 다시 만들었습니다 — 시네마틱 랜딩, IDE 스타일 이력서… 그리고 궁금하다면 걸어 들어갈 수 있는 무언가.

**→ https://broccoliindb.github.io**

</details>
