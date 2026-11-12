# ai-dlc-frontend

**AI-DLC (AI-Driven Development Life Cycle)** 기반의 재사용 가능한 Frontend 개발 자동화 환경이다.

GitHub Issue를 중심으로 AI 에이전트가 설계·구현·리뷰·기록을 수행하고, 사용자가 주요 의사결정과 PR Merge를 담당한다.

프로젝트별 연결 정보는 `ai-dlc.yaml`로 분리하여 공통 개발 워크플로우를 여러 Frontend 프로젝트에 재사용할 수 있다.

## 주요 기능

- **Issue 기반 개발:** Issue 분석부터 설계, 구현, 리뷰, PR까지 단계별 진행
- **역할 분리:** Architect(설계·리뷰), Builder(구현·검증), Reporter(기록)
- **품질 게이트:** 승인 범위, 빌드·린트·기능 검증, 리뷰, CI 상태 확인
- **GitHub Projects 연동:** 작업 선정, 우선순위, 사용자 결정 및 진행 상태 관리
- **공식 명세 참조:** Specs 저장소의 요구사항·API·인증/인가·ERD·UI 계약을 읽기 전용으로 조회
- **개발 기록:** 작업 설계, 진행 로그 및 월별 devlog 관리
- **사용자 승인:** 중요한 결정과 PR Merge는 사용자가 직접 수행

## 디렉터리 구조

```text
ai-dlc-frontend/
├── .agents/
│   └── skills/
│       └── issue-driven-dev/
│           └── SKILL.md
├── .codex/
│   └── agents/
│       ├── architect.toml
│       ├── builder.toml
│       └── reporter.toml
├── docs/
│   ├── devlog/
│   │   └── TEMPLATE.md
│   ├── prompts/
│   │   └── quick.md
│   ├── current-task.md
│   ├── current-work-log.md
│   └── review-checklist.md
├── templates/
│   └── .github/
│       ├── ISSUE_TEMPLATE/
│       │   └── feature_request.yml
│       └── PULL_REQUEST_TEMPLATE/
│           └── pull_request_template.md
├── AGENTS.md
├── ai-dlc.example.yaml
└── README.md
```

## 프로젝트 등록과 실행

이 저장소를 공통 운영 루트로 열고 `projects.example.yaml`을 `projects.local.yaml`로 복사해 프로젝트의 `name`, `aliases`, 절대 `path`를 등록한다. 로컬 등록과 `.idea/`는 Git에서 제외하고 비밀값을 넣지 않는다. 공유 예시에는 실제 로컬 경로를 넣지 않는다.

대상 루트에는 프로젝트별 `ai-dlc.yaml`이 필요하다. 새 설정은 `ai-dlc.example.yaml`을 참고하며 기존 설정이나 작업 기록을 덮어쓰지 않는다. `Ask-Teacher 프로젝트 작업 시작하자`라고 요청하면 Main은 등록 이름·별칭으로 대상을 식별하고 대상 설정·Git 루트와 Frontend 원격을 확인한다. 등록이 없거나 중복되거나 확인할 수 없으면 사용자에게 묻는다. 상세 조건은 공통 `AGENTS.md`를 따른다.

공통 Skill·Agent·리뷰 기준과 템플릿은 이 저장소에서 읽는다. 프로젝트 설정·소스·현재 설계·작업 로그·월별 devlog·Git·검증은 대상 프로젝트에서 처리한다. 대상 `.github` 템플릿을 우선 사용하고 없으면 공통 템플릿을 참고한다. 기존 프로젝트 파일은 자동 복사하거나 삭제하지 않는다.

이 구조는 문서 기반 절차다. 새 Codex 세션에서 공통 Skill 발견과 사용자 정의 역할 호출을 별도로 확인해야 하며 자동 로딩이나 기존 세션의 즉시 갱신을 보장하지 않는다. 필수 역할을 사용할 수 없거나 대상 지시와 충돌하면 중단해 보고한다. 파일 접근 권한과 보호 규칙도 별도로 적용된다.

## 기본 개발 흐름

1. **작업 선정:** GitHub Issue 또는 사용자가 승인한 작업 확인
2. **설계:** Architect가 요구사항·API·UI 명세와 구현 범위를 분석하고 Main이 설계 게이트 확인
3. **구현:** Builder가 승인된 범위에서 컴포넌트·화면·API 연동 구현
4. **검증·리뷰:** 빌드·린트·필요한 자동 테스트 및 화면·Network·Console 검증 후 Architect 독립 리뷰
5. **기록:** Reporter가 검증된 결과를 월별 devlog에 기록
6. **PR 확인:** Main이 CI·리뷰·완료 조건을 확인하고 사용자에게 보고
7. **Merge:** 사용자가 직접 PR을 병합하고 이후 브랜치 정리

세부 절차와 예외·중단 조건은 `AGENTS.md` 및 각 Skill을 따른다.

## 운영 원칙

- 한 번에 하나의 Issue를 기준으로 작업하며, 예외는 사용자의 명시적 승인을 따른다.
- 승인된 작업 범위를 벗어나 임의로 구현하지 않는다.
- 공식 명세의 원본을 임의로 변경하거나 중복 관리하지 않는다.
- 빌드·린트만으로 기능 검증이나 전체 타입 검사가 완료됐다고 판단하지 않는다.
- 화면·API 동작과 관련된 검증은 실제 실행 결과와 미검증 사항을 구분한다.
- PR Merge와 중요한 의사결정은 사용자가 담당한다.
- 토큰·비밀번호·API 키 등 비밀값은 설정 및 작업 기록에 포함하지 않는다.

## 수정 권한과 계약 변경 대응

일반 기능 개발에서는 공식 계약과 AI-DLC 운영 파일을 수정하지 않는다. 공식 원본은 `ai-dlc.example.yaml`의 연결 구조에 따라 실제 `ai-dlc.yaml`에 지정한 Specs 저장소에서 읽는다. 인증/인가 계약은 별도 `contracts.authentication`이 설정되면 해당 항목의 `repository_role`·`path`와 연결된 저장소의 `repository`·`default_branch`가 지정한 원본을 API 계약과 함께 확인한다. 별도 항목이 없을 때만 `contracts.api` 문서의 공식 인증 참조를 따른다. 설정된 원본을 확인할 수 없거나 계약 간 충돌이 있으면 임의로 대체하지 않고 중단해 보고한다.

| 대상 | 일반 개발 사이클 | 변경 절차 |
| --- | --- | --- |
| 공식 요구사항·API·인증/인가·ERD·UI 계약 | 읽기 전용 | 별도 계약 관리 절차 |
| AGENTS·Skill·Agent·설정·운영 문서·템플릿(README.md 제외) | 수정 금지 | 목적·대상·영향에 대한 사용자 승인을 받은 별도 AI-DLC 유지보수 |
| README.md | 승인된 작업 범위에서 수정 가능 | 공식 계약·운영 규칙·승인 조건 변경 수단으로 사용하지 않음 |
| 현재 설계·작업 로그·월별 devlog·Skill이 명시한 상태 파일 | 기존 역할·책임·승인 범위에서 수정 가능 | Main 설계 승인·append-only 등 기존 조건 유지 |

계약 변경이 필요해 보이면 기존 계약으로 승인된 요구사항을 구현할 수 있는지 먼저 검토한다. Main은 대안 검토 근거, 변경 사유·영향 범위·수정안을 사용자에게 보고하고 관련 Issue·PR에 기록한다. 기존 Issue 수정 승인 조건은 유지한다. 계약 변경에 의존하는 부분은 보류하고 별도 계약 관리 절차의 공식 원본 반영을 확인한 뒤 설계 게이트를 다시 판단한다. 기능 구현이나 AI-DLC 유지보수 승인만으로 계약을 수정하지 않는다.

권한의 기준은 `AGENTS.md`, 상세 대응 절차는 `.agents/skills/issue-driven-dev/SKILL.md`를 따른다.

## 공통 AI-DLC 업데이트

공통 운영 파일 변경은 사용자가 승인한 별도 AI-DLC 유지보수에서 지정된 범위만 수행한다. 기존 승인·역할·품질 게이트와 사용자 직접 merge 원칙을 유지한다. 공식 계약 변경은 별도 절차를 따른다.

대상 프로젝트의 기존 운영 파일은 공통 경로와의 차이 및 실제 로딩을 포함한 전환 검증 전까지 삭제하지 않는다. 프로젝트별 설정과 실제 작업 기록도 보존한다. 공통 저장소의 유지보수 이력과 검증 결과는 PR에 기록하며 공통 `docs/current-task.md`와 `docs/current-work-log.md`는 초기 템플릿으로 유지한다.
