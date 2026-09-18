# 설계서 양식 매핑 — TPL-SWE3-001

이 프로젝트의 SW 상세설계서는 반드시 아래 구조(`WP_Templates/Engineering/SoftwareDetailedDesignAndUnitConstruction/TPL-SWE3-001_SW 상세설계서 템플릿.docx`)를 따릅니다.

**중요**: 이 저장소에는 docx를 직접 편집할 수 있는 도구가 없습니다. 작업 방식은 다음과 같습니다.
1. 아래 구조와 동일한 순서/제목으로 Markdown 상세설계서 초안을 작성합니다(Write 도구).
2. 사용자에게 `WP_Templates/Engineering/SoftwareDetailedDesignAndUnitConstruction/TPL-SWE3-001_...docx`를 복사해 `<산출물 ID>_<산출물명>.docx`로 이름을 바꾸고, Markdown 초안 내용을 해당 섹션에 옮겨 담도록 안내합니다 (`WP_Templates/Engineering/README.md`의 절차).
3. UML/호출관계 다이어그램은 `TPL-SWE3-002_상세설계 UML 및 호출관계 템플릿.drawio`를 복사해 작성하도록 안내합니다.
4. 템플릿 ID/적용 산출물 등록은 `PRC-TPL-001_표준 산출물 양식 등록부.xlsx`에 기록하도록 안내합니다 (등록부 파일 자체를 임의로 수정하지 않음).

## 섹션별 매핑

| 템플릿 섹션 | 필수 반영 내용 | 참조 |
|---|---|---|
| 문서 통제 / 변경 이력 / 검토 승인 | 문서 ID, 버전, 작성/검토/승인자, 베이스라인 — 실제 값이 없으면 TBD로 표시 | — |
| 1. 목적 및 적용범위 / 1.1 목적 / 1.2 적용범위 / 1.3 적용 경계 | 대상 아키텍처 요소, 생명주기 단계, 포함·제외 경계 | 입력 아키텍처 설계서(`TPL-SWE2-001`) 명시 |
| **2. 모듈 분해** | 아키텍처 요소 → 구현 단위 분해, ID/책임/소스 위치, 단일 책임(SRP)·응집도 확인 | `module-decomposition.md` |
| **3. 상세 호출관계** | 함수/클래스 호출 순서, 의존 방향, 데이터 흐름, **순환 호출 금지 확인** | `module-decomposition.md` |
| 4. 공통 자료형 | 공통 데이터 구조/열거형/단위/범위/불변조건/직렬화 규칙 | `function-contracts-and-datatypes.md` |
| **5. 핵심 함수 계약** | 함수별 입력/출력/사전조건/사후조건/부작용/예외/시간 제약 | `function-contracts-and-datatypes.md` |
| 6. 핵심 알고리즘 | 처리 순서, 판단 조건, 경계값, 의사코드/흐름도 | `algorithm-and-behavior-design.md` |
| 7. 정책 의사결정표 | 입력 조건 조합, 우선순위, 기대 동작, 충돌 해결 규칙 | `algorithm-and-behavior-design.md` |
| 8. 상태전이 상세 | 상태 저장 위치, 전이 함수, 이벤트, 가드, 타이머, 초기화, 금지 전이 | `algorithm-and-behavior-design.md` |
| 9. Web 및 API 상세 | 엔드포인트, 요청/응답, 검증, 오류 코드, 세션/보안 경계 (해당 시) | `algorithm-and-behavior-design.md` |
| **10. 오류와 방어 동작** | 검출 위치, 전파 차단, 기록, 복구, 유효하지 않은 입력 대응 | `defensive-programming-and-error-handling.md` |
| 11. 코딩 및 검증 규칙 | 코딩 표준, 정적분석, 단위검증 커버리지 기준, 리뷰 기준 | `coding-and-verification-rules.md` |
| 12. 단위와 요구사항 할당 | 구현 단위 ↔ 아키텍처 ↔ SW 요구사항 ↔ 단위시험 할당, 누락 확인 | `coding-and-verification-rules.md` |
| 13. 구현 경계 | 생성 코드, 외부 라이브러리, 플랫폼 종속부, 미구현 범위, SBOM 연계 | `coding-and-verification-rules.md` |
| 14. 추적성 | 아키텍처 ↔ 상세설계 ↔ 소스 ↔ 함수 ↔ 단위시험 양방향 연결 | `coding-and-verification-rules.md`, `TPL-TRC-001` |
| 15. 참고자료 | 입력 아키텍처 설계서, 코딩규칙, API 문서, 외부 라이브러리 자료 | — |

## 작성 순서 권장

1. 입력 아키텍처 설계서(`TPL-SWE2-001`)와 대상 컴포넌트 확인 (1장)
2. 모듈 분해 및 호출관계 정의 (2~3장) — SRP/응집도/순환 없음 확인
3. 공통 자료형 정의 (4장)
4. 핵심 함수 계약 정의 (5장) — 아키텍처 인터페이스 계약과의 일관성 확인
5. 알고리즘/정책결정표/상태전이/API 상세 (6~9장, 해당하는 것만)
6. 오류와 방어 동작 설계 (10장)
7. 코딩/검증 규칙, 요구사항 할당, 구현 경계 (11~13장)
8. 추적성, 참고자료 정리 (14~15장)
