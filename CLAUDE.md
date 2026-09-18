# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 소프트웨어 개발 정보
이 소프트웨어 개발은 다음의 항목을 이용해서 개발한다.

- python 3.14를 이용해서 개발한다.
- 코드 테스트는 unittest를 활용한다.
- 순환 복잡도, 함수라인수 등 측정 지표는 오픈소스 도구를 이용한다.

## 프로젝트 개발 정책

### 개발 생명 주기

- 이 프로젝트는 **반드시** 분석, 설계, 구현, 테스트의 순서로 개발을 진행한다.
- 각 단계가 완료되었을 때, 지정된 템플릿을 이용한 산출물이 생성되어야 한다.

### 분석 지침
- 요구사항 분석 단계 수행은 requirements-analyst 서브에이전트가 담당한다.

### 아키텍처 설계 지침
- 아키텍처 설계(A-SPICE SWE.2) 단계 수행은 architecture-designer 서브에이전트가 담당한다.

### 상세설계 지침
- 상세설계(A-SPICE SWE.3) 단계 수행은 detailed-designer 서브에이전트가 담당한다.
- 승인된 아키텍처 설계서(architecture-designer 산출물)의 컴포넌트 경계와 인터페이스 계약을 입력으로 하며, 이를 임의로 변경하지 않는다.

### 구현 지침
- 구현 단계 수행은 coding 서브에이전트가 담당한다.
- TDD 방식으로 진행하고, TDD 스킬을 사용해야 한다.
- 다음의 품질 지표를 **반드시** 준수해야한다.
  - 함수 라인수는 순수코드라인 50라인 이하여야 한다.
  - 함수 순환복잡도는 10 이하여야 한다.
  - 중복 코드는 7라인 까지 허용한다.
  - 주석은 doxygen 방식으로 작성하며, 20% 이상 작성해야 한다.
- 함수명, 변수명은 calmel case를 사용한다.
- 구현과 동시에 작성되는 유닛(함수/모듈) 수준 테스트(A-SPICE SWE.4에 해당)는 이 단계의 TDD 사이클 안에서 coding 서브에이전트가 담당하며, 단위 테스트는 TDD로 대체한다.
- 단위 테스트는 Branch 커버리지 100%를 달성해야 하며, 테스트 성공률은 100%여야 한다.

### 테스트 지침

개발 생명주기의 "테스트" 단계는 검증 범위에 따라 두 서브에이전트로 나뉜다. 둘 다 유닛(함수/모듈) 내부 로직은 다루지 않으며, 그 부분은 구현 지침의 TDD 사이클이 담당한다.

- **통합시험(A-SPICE SWE.5)**: integration-tester 서브에이전트가 담당하며, `integration-testing` 스킬을 사용해야 한다. 테스트 베이시스는 아키텍처 설계서의 인터페이스 명세와 통합 순서이며, 함수 커버리지·Call 커버리지 100% 달성을 목표로 하고, 테스트 성공률은 100%여야 한다.
- **자격시험/시스템 테스트(A-SPICE SWE.6)**: sw-system-tester 서브에이전트가 담당하며, `sw-system-test` 스킬을 사용해야 한다. 테스트 베이시스는 SW 요구사항 명세서이며, 소프트웨어 내부 구조를 알지 못해도 설계 가능한 블랙박스 관점의 기능/비기능 테스트 케이스를 다루고, 테스트 성공률은 100%여야 한다.

### 감사/점검 지침
- 위 각 단계 산출물이 A-SPICE CL2(Capability Level 2) 기준(계획/모니터링/식별/버전관리/리뷰/추적성)을 만족하는지에 대한 감사는 aspice-cl2-auditor 서브에이전트가 담당하며, 위 서브에이전트들의 업무 범위가 아니다.

## Git 브랜치 정책

AI(서브에이전트)를 활용한 개발("바이브 코딩")의 결과물은 반드시 아래 정책을 따라 GitHub에 반영한다.

### 브랜치 전략 (트렁크 기반)

- `main` 브랜치는 항상 배포 가능한 상태를 유지하는 보호 브랜치이며, **직접 커밋/푸시를 금지**한다. 모든 변경은 Pull Request(PR)를 통해서만 병합한다.
- 작업은 `main`에서 분기한 짧은 수명의 브랜치에서 진행하고, 목적에 따라 접두어를 붙인다:
  - `feature/<설명>` — 신규 기능
  - `fix/<설명>` — 버그 수정
  - `docs/<설명>` — 문서/스킬/에이전트 설정 변경
  - `chore/<설명>` — 빌드/CI/의존성 등 기타 변경
- 브랜치는 하나의 작업 단위(요구사항/설계/구현/테스트 산출물 등)에 대응하도록 작게 유지하고, 병합 후 즉시 삭제한다.
- 병합 방식은 **Squash and merge**를 기본으로 해, AI 에이전트가 반복 커밋한 중간 이력을 `main`에서는 하나의 논리적 커밋으로 정리한다.
- `main`으로의 force-push, 브랜치 삭제는 금지한다.

### PR(Pull Request) 규칙

- PR은 개발 생명주기 단계(분석/설계/구현/테스트) 중 하나 이상의 산출물 변경을 명확히 설명해야 하며, 어떤 서브에이전트가 산출물을 생성/수정했는지 PR 설명에 명시한다.
- PR은 아래 "CI/CT" 워크플로우(GitHub Actions)의 모든 상태 검사(status check)를 통과해야 병합할 수 있다.
- 병합 전 브랜치를 `main` 최신 상태로 갱신(rebase 또는 update branch)한다.

### GitHub 저장소 branch protection 설정 (저장소 관리자가 1회 설정)

이 정책을 GitHub에서 실제로 강제하려면 저장소 Settings → Branches → `main`에 대한 branch protection rule(또는 Rulesets)을 아래와 같이 설정해야 한다. 이 설정은 Claude Code가 대신 적용할 수 없으므로(저장소 관리자 권한 필요) 사용자가 직접 적용한다.

- Require a pull request before merging (직접 push 금지)
- Require status checks to pass before merging — `Lint & Static Analysis`, `Test & Coverage` (`.github/workflows/ci.yml`의 job)를 필수 체크로 지정
- Require branches to be up to date before merging
- Do not allow force pushes / Do not allow deletions (main 대상)
- (팀 규모가 커지면) Require approvals — 1인 바이브 코딩 단계에서는 생략 가능, 협업 시작 시 최소 1명으로 상향

### 지속적 통합/지속적 테스트(CI/CT) — GitHub Actions

- `.github/workflows/ci.yml`이 PR 생성/갱신 시(및 `main` 병합 후) 자동 실행된다.
- **지속적 통합(CI)**: `radon`으로 순환복잡도(10 이하)와 함수 라인수를 점검하고, `pylint`로 중복 코드(7라인 초과 금지)를 점검한다 (구현 지침의 품질 지표와 동일 기준).
- **지속적 테스트(CT)**: `coverage run --branch -m unittest discover`로 전체 단위 테스트를 실행하고, `coverage report --fail-under=100`으로 Branch 커버리지 100%를 강제한다 — 테스트 실패 또는 커버리지 미달 시 워크플로우가 실패하여 PR 병합이 차단된다(브랜치 보호 규칙과 연동 시).
- 아직 Python 소스/테스트가 없는 PR(문서/설정 변경 등)은 해당 단계를 건너뛰고 통과 처리된다.