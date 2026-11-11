## 변경 요약

-

## 관련 Issue

- Closes # (연결 Issue가 없는 명시적 예외 작업은 승인 근거 기록)

## PR 방향

- base: `ai-dlc.yaml`의 `repositories.frontend.default_branch`
- compare: `ai-dlc.yaml`의 `development.branch_pattern`

## 테스트 결과

- [ ] `development.build_command` 통과
- [ ] `development.lint_command` 통과
- [ ] 변경 API·Page·Component 및 주요 사용자 흐름 검증
- [ ] 테스트 계정·인증 정보·API 대상·파일 격리 확인
- 테스트 범위 / 실행 결과:
- 화면·Network·Console 결과:
- 미검증 사항 / 해당 없음의 이유 / 명세 불일치:

## 리뷰 포인트

-

## 이번 PR에서 제외한 범위

-

## 체크리스트

- [ ] docs/current-task.md 범위 안에서 작업했다.
- [ ] 작업 브랜치와 PR base가 설정된 브랜치 규칙을 따른다.
- [ ] `ai-dlc.yaml`의 활성 서비스 `requirements`, `contracts.api`, `contracts.erd`, `contracts.ui`가 지정한 공식 원본과 관련 변경이 일치한다.
- [ ] 요청·응답 타입과 UI 입력값, API·Component 책임을 구분했다.
- [ ] 입력 검증·상태·오류·캐시 갱신·라우팅·권한을 확인했다.
- [ ] 실제 검증 결과와 미검증 사항을 구분했다.
- [ ] Architect 독립 리뷰의 필수 수정을 반영·재검증했다.
- [ ] Reporter devlog 반영과 최종 head의 CI·필수 리뷰·보호조건·충돌을 확인했다.
