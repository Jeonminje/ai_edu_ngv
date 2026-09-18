# 산출물 양식 매핑 — TPL-SWE5-001/002/003

이 프로젝트의 소프트웨어 통합시험 산출물은 반드시 아래 3개 사내 양식(`WP_Templates/Engineering/SoftwareComponentVerificationAndIntegrationVerification/`)을 따릅니다.

**중요**: 이 저장소에는 docx/xlsx를 직접 편집할 수 있는 도구가 없습니다. 동일한 구조의 Markdown/표 초안을 작성한 뒤, 사용자에게 템플릿을 복사해 실제 파일로 옮기도록 안내합니다 (`WP_Templates/Engineering/README.md` 절차: 템플릿 복사 → `<산출물 ID>_<산출물명>` 로 이름 변경 → `PRC-TPL-001` 등록부에 기록).

## TPL-SWE5-001 (통합전략 및 통합시험 명세서, 14개 장)

| 섹션 | 필수 반영 내용 | 참조 |
|---|---|---|
| 1 (1.1~1.3) 목적/적용범위/경계 | 대상 SW 구성요소, 입력 아키텍처 설계서 버전 | — |
| 2. 통합 원칙 | 통합 단위/단계/위험 우선순위/실패 격리 원칙 | `test-basis-and-order.md` |
| **3. 통합 항목과 순서** | 아키텍처 설계서 11장과 **동일한** 통합 순서, 선행조건/의존성/담당 | `test-basis-and-order.md` |
| 4. 환경 및 형상 | 도구(측정 도구 포함), 실행환경, SW 버전, 시험 데이터, 형상 식별 | `structural-coverage-100.md`의 도구 목록 명시 |
| 5. 진입/종료 기준 | **종료 기준에 함수 커버리지 100% + Call 커버리지 100% 명시** | `structural-coverage-100.md` |
| 6. 통합시험 케이스 요약 | 인터페이스 ID별 시험 ID, 추적, 기대결과, 자동화 여부 | `test-basis-and-order.md` |
| **7. 시험 설계기법** | 통합시험 방법(요구사항기반/인터페이스/결함주입 등) + 케이스 도출 기법(동등분할/경계값/결정테이블/상태전이/오류추정) 선정 근거 | `iso26262-integration-test-methods.md` |
| 8. 실행 및 결과 기록 규칙 | 실행 식별자, 환경, 증거 위치, 결함 연결 방법 | — |
| 9. 회귀 전략 | 변경 영향 기반 회귀 범위, 자동 실행/비교 방법 | — |
| 10. 실패 및 편차 처리 | 실패/환경문제/계획편차 분류·보고·재시험·승인 절차 | — |
| 11. 추적성과 보고 | 아키텍처 인터페이스↔시험케이스↔실행결과↔결함↔보고서 연결 | — |
| 12. 적용 한계 | 통합시험으로 확인 못하는 범위(시스템/HIL/차량/양산) | — |
| 13. 추적성 | 입력 설계/형상, 시험 명세, 결과, 결함의 양방향 추적 | `TPL-TRC-001`과 연계 |
| 14. 참고자료 | 아키텍처 설계서, 상세설계서, 검증 계획, 사용 도구 자료 | — |

## TPL-SWE5-002 (통합시험 케이스, 시트: Integration Cases)

| 컬럼 | 반드시 채울 내용 |
|---|---|
| Test ID | 고유 식별자 |
| Trace | 대응 인터페이스 ID (아키텍처 설계서 6장) — 이 프로젝트에서는 **필수, 공란 불가** |
| Integration Item | 이 케이스가 속한 통합 단계/항목 (3장의 통합 순서와 일치) |
| Stimulus | 입력/호출 조건 (사용 기법에 따라 도출된 구체값) |
| Expected Result | 인터페이스 계약(사후조건/오류계약)에 근거한 기대 결과 |
| Technique | `iso26262-integration-test-methods.md`의 기법 명칭 그대로 기입 |
| Automation | 자동화 여부 (자동화된 경우 실행 스크립트 경로 명시 권장) |

## TPL-SWE5-003 (통합시험 결과서)

**Integration Results 시트**

| 컬럼 | 반드시 채울 내용 |
|---|---|
| Test ID | `TPL-SWE5-002`의 Test ID와 일치 |
| Trace | 대응 인터페이스 ID |
| Result | Pass/Fail/Blocked 등 |
| Actual Result | 실제 관측 결과 |
| Evidence Locator | 로그/커버리지 리포트 등 증거 위치 (함수/Call 커버리지 리포트 경로 포함) |
| Defect ID | 실패 시 결함 추적 ID |
| Disposition | 재시험/면제/승인 등 처리 결과 |

**Run Summary 시트**: Run ID, Date, Baseline, Environment, Planned/Pass/Fail 건수, Overall 판정, Limitation(한계사항)을 기록하며, Overall 판정에는 **함수 커버리지/Call 커버리지 실측치가 100%에 도달했는지 여부**를 반드시 포함합니다.

## 작성 순서 권장

1. 아키텍처 설계서 확인 → 인터페이스/통합 순서 추출 (`test-basis-and-order.md`)
2. `TPL-SWE5-001` 1~5장(목적, 원칙, 통합 순서, 환경, 진입·종료 기준) 작성 — 종료 기준에 커버리지 100% 명시
3. 시험 설계기법 선정 (7장) → `TPL-SWE5-002` 케이스 작성
4. 시험 실행 → `structural-coverage-100.md` 절차로 함수/Call 커버리지 실측
5. 갭 발견 시 케이스 추가 → 재실행 → 갭 해소까지 반복
6. `TPL-SWE5-003` 결과서 작성, 추적성(11/13장) 및 참고자료(14장) 정리
