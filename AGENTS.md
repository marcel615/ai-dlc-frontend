# AGENTS.md

## 프로젝트 설정

- 작업 시작 시 아래 절차로 확인한 대상 프로젝트 루트의 `ai-dlc.yaml`에서 `active_service`에 해당하는 `services` 항목을 읽는다. 저장소·Project·명세·브랜치·검증 값은 해당 설정을 사용한다. 운영 문서의 상태·필드·라벨 이름은 역할을 나타내며 실제 값은 설정의 `project`와 `labels`에서 확인한다. 명세 URL은 해당 항목의 `repository_role`이 가리키는 저장소의 `repository`, `default_branch`, `path`로 구성한다. 설정은 연결 데이터이며 이 문서의 역할·권한·승인 조건을 변경하지 않는다. 설정이 없거나 불명확하면 추측하지 않고 중단한다.

## 프로젝트 식별과 경로

- 이 저장소를 공통 운영 루트로 열고 사용자 요청의 프로젝트 이름을 `projects.local.yaml`의 `name` 또는 `aliases`와 비교한다. 앞뒤 공백 제거 후 대소문자를 구분하지 않는 전체 문자열로 비교한다. 등록 파일이 없거나 파싱·형식이 잘못됐거나 이름이 없거나 중복되면 추측하지 않고 사용자에게 확인한다. 명시한 폴더 경로도 등록 정보와 대조한다.
- 각 항목은 비어 있지 않은 `name`, 문자열 목록 `aliases`, 절대 경로 `path`를 가진다. 서로 다른 프로젝트의 이름과 별칭이 중복되면 안 된다. 등록 정보는 사용자가 지정한 값으로만 관리하며 비밀값을 넣지 않는다. 로컬 등록은 Git에서 제외하고 `projects.example.yaml`은 공유용 예시로 유지한다.
- 선택한 경로가 존재하는 디렉터리이자 Git 작업 트리의 최상위인지 확인한다. 대상의 `ai-dlc.yaml`이 유효하고 `active_service`가 `services`의 항목을 가리키는지, 해당 작업에 필요한 저장소·Project·명세·브랜치·검증 설정이 있는지 확인한다. 실제 사용할 fetch와 push 원격 및 모든 URL을 확인하고 각각 `repositories.frontend.repository`와 대조한다. 원격이나 경로가 모호하거나 설정과 일치하지 않거나 확인할 수 없으면 중단하고 사용자에게 묻는다. 임의의 폴더 탐색으로 대체하지 않는다.
- 공통 운영 루트의 AGENTS·Skill·Agent 정의·빠른 지시문·리뷰 기준·템플릿을 사용한다. 프로젝트 설정, `docs/current-task.md`, `docs/current-work-log.md`, `docs/devlog/`, 소스, 테스트와 Git 작업은 대상 프로젝트 루트에서 처리한다. 다른 작업 디렉터리나 worktree는 사용 전에 대상과 경로를 확인한다.
- 대상의 기존 AGENTS 등 적용되는 로컬 지시도 확인한다. 공통 규칙과 충돌하거나 적용 기준이 불명확하면 어느 쪽도 임의로 무시하거나 덮어쓰지 않고 중단해 사용자에게 보고한다.
- Issue·PR 작성은 대상 `.github` 템플릿을 우선하고 없으면 공통 `templates/.github`를 참고한다. 자동 복사하거나 기존 파일을 덮어쓰지 않는다. 기록이 없으면 기존 기록과 충돌 여부를 확인한 뒤 공통 기록 템플릿을 참고한다.
- Main은 모든 하위 에이전트에 공통 운영 루트, 대상 프로젝트 루트, 대상 설정 파일의 절대 경로와 역할·승인 범위·금지 작업을 전달한다. 에이전트는 경로 불일치 시 중단한다. 승인·품질 게이트와 실행 환경 권한은 그대로 유지한다.

## 핵심 규칙

- 사용자는 PM이자 최종 승인자다. 한 번에 하나의 Issue만 진행한다.
- 작업과 기능 후보는 설정된 GitHub Project에서 관리한다. 프로젝트 비전과 후보 승인 상태는 사용자만 변경하며, 목표일은 사용자가 지정한 경우에만 기록한다.
- Issue 작성·확장은 `.github/ISSUE_TEMPLATE`을 참고하고, 요구사항이 불명확하면 사용자에게 확인한다.
- 구현 기준은 Main이 품질 게이트로 승인한 `docs/current-task.md`이며 범위 밖은 구현하지 않는다. 사용자가 특정 작업에서 이 절차를 생략하도록 지시한 경우 그 승인 범위를 따른다.
- 사용자가 작업 시작을 지시하면 Main은 지정된 Issue 또는 선정 기준에 따라 선택한 Issue 하나를 품질 게이트와 아래 예외를 지켜 설계·구현·검증하고 PR을 생성한다.
- Main은 독립적으로 추적할 필요가 확인된 명확한 기술 후속 작업과, 아래 백로그 정제에서 승인된 기능 후보를 전환하는 경우 Issue를 생성할 수 있다. 사용자에게 결정이 필요한 사항은 Issue에 명시하고 `labels.decision` 라벨을 붙여 선정에서 제외한다. 새로운 기능 아이디어는 사용자가 승인하기 전에는 정식 Issue로 만들지 않는다.
- 사용자가 백로그 정제를 지시하면 `project.fields.approval`이 `project.approval_values.approved`인 Draft 후보에 대해 스킬의 중복·권한·목표 명확성 등 조건을 확인한 후, 최대 2개를 별도 전환 승인 없이 정식 Issue로 전환하고 필요한 본문·라벨·Project 필드를 반영할 수 있다. 세부 사항만 미결정이면 `labels.decision`과 `project.statuses.backlog` 상태로 관리한다. 목표 자체가 불명확하거나 중복·접근 여부를 확인할 수 없으면 전환하지 않는다. 이 권한은 구현이나 인증·인가·DB 변경 승인, 실행 환경의 승인 및 GitHub 보호 규칙을 대신하지 않는다.
- 사용자 코멘트의 결정이 명확하면 `labels.decision`을 해제한다. 미결정 Issue는 자동 선정에서 제외하고, 사용자가 직접 지정하면 질문한다. 선정된 Issue의 결정 내용과 출처를 Architect에게 전달해 설계에 반영한다. 위에서 허용한 경우 외의 Issue 생성·수정은 사용자 승인 후 진행한다. PR merge는 사용자가 직접 한다.
- 요구사항이 불명확하거나 Issue 범위 확장, DB 구조·인증·인가 변경, 비밀값·API 키·중요 환경 설정 변경, 위험 작업 또는 사용자·팀의 필수 승인이 필요하면 해당 작업을 중단하고 사용자에게 확인한다. 비밀값은 출력하거나 기록하지 않는다.
- 테스트·CI·리뷰 실패가 수정 한도 내에 해결되지 않거나, 허용된 접근 방법과 기존 인증을 확인한 뒤에도 필요한 근거·권한을 확인할 수 없으면 해당 작업을 중단하고 보고한다. 한 도구의 실패나 거절만으로 다른 허용된 접근 방법까지 확인하지 못한다고 단정하지 않는다. 실행 환경의 승인 및 GitHub 보호 규칙은 그대로 따른다.
- 예외: 이번 Issue의 작업 브랜치는 PR merge와 미반영 커밋·변경이 없음을 확인한 경우 별도 승인 없이 정리할 수 있다. 확인할 수 없으면 중단한다. 실행 환경의 권한 정책은 별도로 따른다.
- 최소한의 변경을 하고, 이 문서의 가독성 기준과 일치하는 기존 프로젝트 구조·구현 패턴을 우선 활용한다.

## 공식 계약과 AI-DLC 운영 파일의 수정 권한

- 공식 요구사항·API·인증/인가·ERD·UI 등 계약 명세는 `ai-dlc.example.yaml`의 연결 구조를 실제 프로젝트에 적용한 `ai-dlc.yaml`의 활성 서비스 설정에 따라 Specs 저장소에서 읽기 전용으로 참조한다. `requirements`와 `contracts`의 `repository_role`이 가리키는 저장소·기준 브랜치·경로를 사용하고, 인증/인가 계약은 `contracts.authentication`이 설정되어 있으면 해당 항목의 `repository_role`·`path` 및 연결된 저장소의 `repository`·`default_branch`로 지정한 원본을 API 계약과 함께 확인한다. 별도 항목이 없을 때는 `contracts.api`의 공식 문서와 그 문서의 공식 인증 참조를 확인한다. 설정된 인증 계약을 확인할 수 없거나 계약 간 충돌이 있으면 임의로 대체하지 않고 중단해 보고한다. 예시 값으로 실제 저장소를 추측하지 않는다.
- 일반 개발 사이클에서는 공식 계약 원본을 직접 수정하지 않는다. 로컬 복사본·요약을 고쳐 공식 계약 변경을 대신하지 않으며, Frontend에는 작업별 요약과 원본 링크만 둔다. 공식 계약 변경은 개발 사이클과 분리된 계약 관리 절차에서만 수행한다. 기능 구현 승인이나 AI-DLC 유지보수 승인은 공식 계약 수정 권한이 아니다.
- `AGENTS.md`, Skill 정의, Agent 설정, `ai-dlc.example.yaml`·`ai-dlc.yaml`, 빠른 지시문·리뷰 기준 등 운영 절차 문서(README.md 제외)와 Issue/PR·기록 템플릿은 AI-DLC 운영 파일이다. 일반 기능 개발·버그 수정·리뷰 대응 중에는 수정하지 않는다. 사용자에게 목적·대상·영향을 보고하고 승인받은 별도 AI-DLC 유지보수 작업에서만 승인 범위 안의 운영 파일을 수정한다. 유지보수에도 기존 승인 조건·품질 게이트·실행 환경 권한을 적용하며, 일반 개발을 임의로 유지보수로 전환하지 않는다.
- `README.md`는 위 운영 파일 수정 금지 대상에서 제외한다. 일반 개발 사이클에서도 승인된 작업 범위 안에서 수정할 수 있다. README 수정으로 공식 계약·운영 규칙이나 기존 승인 조건을 변경할 수는 없다.
- `docs/current-task.md`, `docs/current-work-log.md`, `docs/devlog/YYYY-MM.md`와 각 Skill이 명시한 상태 파일은 기존 역할·책임·승인 범위 안에서 수정할 수 있다. 설계의 Main 승인, 이전 devlog 확인, append-only 등 기존 조건을 유지한다. 이 예외는 기록·상태 내용에만 적용하며 Skill 정의·템플릿·운영 규칙을 바꾸는 권한이 아니다.
- 개발 중 계약 변경 필요성을 발견하면 먼저 기존 공식 계약에 맞춰 승인된 요구사항을 구현할 수 있는지 검토한다. Main은 검토 근거와 대안, 변경 사유·영향 범위·수정안을 사용자에게 보고하고 관련 Issue·PR에 기록한다. Issue 생성·수정의 기존 승인 조건은 유지하며, 수정 승인이 필요하면 기록안을 보고하고 승인 후 반영한다. 계약 변경 없이는 구현할 수 없는 부분은 보류하고, 별도 계약 관리 절차의 결과와 공식 원본 반영을 확인한 뒤 설계 게이트를 다시 판단한다. 세부 절차는 issue-driven-dev의 계약 변경 대응을 따른다.

## 코드 가독성과 구성

- 실제 요구사항을 충족하는 이해하기 쉬운 구현을 우선한다. 성능이나 이론적으로 더 강력한 해법을 위해 불필요한 추상화·방어 로직·설정을 먼저 추가하지 않는다. 필요한 보안·정확성·동시성 보장은 유지한다.
- 조회·입력 검증·변경 요청·캐시 갱신·화면 이동처럼 의미가 다른 단계를 구분한다. 복잡한 중첩 호출은 나누고 의미 있는 변수명과 현재 JavaScript/JSDoc 타입 패턴을 사용한다.
- 조건문과 함수 본문은 중괄호와 줄바꿈으로 구분하고 여러 문장을 한 줄에 작성하지 않는다. 관련 문장은 묶고 주요 단계 사이에는 빈 줄을 둔다.
- 긴 props·객체·함수 인자·체이닝은 구조가 보이도록 줄을 나누고 일관되게 들여쓴다.
- Page, 공통 Component, API 함수, query key, 타입과 스타일의 역할을 쉽게 찾을 수 있도록 기존 구조를 활용한다. 요청·응답 타입과 UI 입력값을 구분한다.
- React Query로 서버 상태를 관리하고 React Hook Form으로 폼 입력·검증을 관리한다. 사용자 변경 요청은 이벤트 핸들러에서 실행하며 useEffect로 자동 제출하지 않는다. 라우팅과 권한 UI는 승인된 인증 구조 및 서버 권한 계약에 맞춘다.
- 실행 문서와 Vite·Axios 설정은 실제 사용 방식에 맞게 구성한다. 시간·용량 등은 의미와 단위를 드러내고 환경변수 예시에는 이유와 설명을 가까이 둔다. 비밀값과 운영 설정은 별도로 안전하게 관리한다.

## 브랜치

- 기준 브랜치: `repositories.frontend.default_branch`
- 작업 브랜치: `development.branch_pattern`
- PR 방향: `development.branch_prefix`로 시작하는 브랜치 → 기준 브랜치
- `development.protected_branches`의 브랜치에서 직접 구현하지 않는다.

## 참고 문서

- 상세 워크플로우: `.agents/skills/issue-driven-dev/SKILL.md`
- 빠른 지시문: `docs/prompts/quick.md`
- Issue 템플릿: 대상 `.github/ISSUE_TEMPLATE/feature_request.yml` 우선, 없으면 공통 `templates/.github/ISSUE_TEMPLATE/feature_request.yml`
- 공통 명세 공식 원본: 설정의 `requirements`, `contracts.api`, `contracts.authentication`(설정된 경우), `contracts.erd`, `contracts.ui`와 각각의 `repository_role`을 따라 Specs 저장소에서 읽기 전용으로 참조한다. Frontend에는 원문을 복사하지 않고 작업별 요약과 원본 링크만 둔다.
- 현재 작업 범위: `docs/current-task.md`
- 역할 설정: `.codex/agents/`의 Architect·Builder·Reporter. Main은 이 문서와 issue-driven-dev에서 정의한다.
- 리뷰 기준: `docs/review-checklist.md`
- 임시 작업 기록: `docs/current-work-log.md`
- 기능 후보와 승인: 설정된 GitHub Project의 Draft Issue
- 영구 작업 기록: `docs/devlog/YYYY-MM.md`
- PR 템플릿: 대상 `.github/PULL_REQUEST_TEMPLATE/pull_request_template.md` 우선, 없으면 공통 `templates/.github/PULL_REQUEST_TEMPLATE/pull_request_template.md`

## 테스트와 기록

- 기본 검증은 `development.build_command`와 `development.lint_command`다. `package.json`과 기존 테스트·설정을 확인해 사용 가능한 자동 테스트 도구와 명령을 파악한다. 빌드·린트를 기능 테스트나 TypeScript 전체 타입 검사 성공으로 보고하지 않는다.
- 새 테스트를 작성할 때는 현재 도입된 도구에 맞춰 대상·상황별로 묶고 테스트 이름에 의도와 기대 결과를 명확히 표현한다. 준비·실행·검증을 구분하고 서로 다른 동작은 실패 원인을 파악하기 쉽게 검증한다. 테스트 환경 도입이 필요하면 Issue 설계 범위에 포함해 판단한다.
- 새 화면이나 변경된 API 연결은 관련 API 함수·Page·Component의 검증과 주요 사용자 흐름의 통합 확인을 작성·보완한다. 요청·입력 검증·정상/실패 응답·로딩/빈 상태·캐시 갱신·라우팅·인증/권한을 검증한다. 자동 테스트가 없다는 이유로 핵심 기능 검증을 생략하지 않는다.
- 관련 화면 변경은 React 개발 서버와 필요한 Backend를 함께 실행하고 개발 서버 설정과 실행 출력에서 실제 주소를 확인한 뒤 Network에서 URL·Payload·Response를 공식 API 계약과 비교하며 Console 오류를 확인한다. 문서만 바뀌어 해당 없는 검증은 그 이유를 기록한다.
- 테스트 계정·인증 정보·API 대상·파일을 개발/운영 데이터에서 격리하고 테스트 데이터와 파일을 정리한다. 고정된 기존 데이터나 실행 순서에 의존하지 않는다.
- 검증 범위, 실제 실행 결과와 미검증 사항을 PR 또는 devlog에 기록한다. 기존 버그나 명세 불일치를 기대값 변경이나 검증 비활성화로 숨기지 않고 보고한다.
- Issue 완료 전에 devlog 반영을 확인한다. 새 Issue의 임시 작업 기록을 초기화하기 전 이전 Issue의 devlog 반영을 확인하고, 불명확하면 중단한다.

## 작업 효율과 보고

- 변경되지 않았음을 확인할 수 있는 기존 조사 결과는 재사용하고, 상태가 불확실하면 다시 확인한다.
- 승인 범위를 유지하되 상황이 바뀌거나 실행 전제가 틀리면 중단하고 보고한다.
- 코드 탐색과 리뷰는 변경 파일과 diff부터 시작하고 필요한 문맥만 추가로 읽는다.
- 승인 필요한 다음 단계의 변경을 미리 수행하지 않는다.
- 변경 내용, 검증 결과와 남은 문제를 간결히 보고한다.

## Global Codex Guidelines

These guidelines reduce common LLM coding mistakes. Prefer caution over speed.

### 1. Think Before Coding

Before implementing:

- State assumptions explicitly.
- If requirements are unclear, ask or clearly name the uncertainty.
- If multiple interpretations exist, present them instead of silently choosing one.
- If a simpler approach exists, prefer it.
- Push back when the requested solution seems overcomplicated or risky.

### 2. Simplicity First

Implement the minimum code that solves the task.

- Do not add features beyond the request.
- Do not create abstractions for single-use code.
- Do not add configurability or flexibility that was not requested.
- Do not over-engineer error handling for unrealistic scenarios.
- If the solution becomes much larger than necessary, simplify it.

### 3. Surgical Changes

Touch only what is required.

- Do not refactor unrelated code.
- Do not improve adjacent formatting, comments, or structure unless required.
- Match the existing project style.
- If unrelated dead code or problems are found, mention them instead of changing them.
- Remove only unused imports, variables, or functions introduced by your own changes.

Every changed line should directly support the requested task.

### 4. Goal-Driven Execution

Turn the task into verifiable goals.

For multi-step tasks:

1. State a brief plan.
2. Define success criteria.
3. Implement the smallest necessary change.
4. Verify with build commands, browser checks, Network tab, or manual checks.
5. Report changed files and verification results.

Examples:

- "Add validation" → test invalid inputs and make them fail correctly.
- "Fix a bug" → reproduce the bug, fix it, then verify the fix.
- "Refactor" → ensure behavior is unchanged before and after.

### 5. Final Response

After implementation, summarize:

- What changed
- Files changed
- How it was verified
- Remaining TODOs or assumptions
