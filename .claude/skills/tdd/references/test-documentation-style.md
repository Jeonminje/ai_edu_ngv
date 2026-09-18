# 테스트 함수 문서화 규칙 (사용 기법 / 긍정·부정 케이스 / Doxygen 목적 주석)

이 프로젝트의 모든 테스트 함수는 아래 3가지를 **반드시** 함께 기술합니다. 셋 중 하나라도 없으면 완료로 보고하지 않습니다.

1. **Doxygen 형식의 테스트 목적 설명** — 이 테스트가 무엇을 검증하는지
2. **사용한 테스트 설계 기법** — 어떤 방법으로 이 테스트 케이스를 도출했는지
3. **긍정(Positive)/부정(Negative) 케이스 여부** — 정상 동작을 검증하는지, 오류/예외 상황을 검증하는지

## 표기 형식

`##` Doxygen 블록에 `@brief`(필수)와 함께 커스텀 태그 `@technique`, `@case`를 사용합니다. 필요하면 상세설계 근거를 `@designRef`로 덧붙입니다.

```python
class TestIsWithinRange(unittest.TestCase):
    ## @brief 입력값이 유효 범위 내부에 있으면 True를 반환하는지 검증한다.
    #  @technique 동등분할 (유효 범위 내부 클래스)
    #  @case 긍정 (Positive)
    #  @designRef DD-VALID-01 (상세설계 5장 함수 계약)
    def testReturnsTrueWhenValueWithinRange(self):
        self.assertTrue(isWithinRange(5, 0, 10))

    ## @brief 입력값이 최소/최대 경계값과 정확히 같을 때도 True를 반환하는지 검증한다.
    #  @technique 경계값분석 (하한/상한 경계)
    #  @case 긍정 (Positive)
    #  @designRef DD-VALID-01
    def testReturnsTrueAtBoundaryValues(self):
        self.assertTrue(isWithinRange(0, 0, 10))
        self.assertTrue(isWithinRange(10, 0, 10))

    ## @brief 입력값이 유효 범위를 벗어나면 False를 반환하는지 검증한다.
    #  @technique 동등분할 + 경계값분석 (범위 밖 클래스, 경계값 인접)
    #  @case 부정 (Negative)
    #  @designRef DD-VALID-01
    def testReturnsFalseOutsideRange(self):
        self.assertFalse(isWithinRange(-1, 0, 10))
        self.assertFalse(isWithinRange(11, 0, 10))

    ## @brief 숫자형이 아닌 입력이 들어오면 TypeError를 발생시키는지 검증한다.
    #  @technique 오류추정 (잘못된 타입 입력)
    #  @case 부정 (Negative)
    #  @designRef DD-VALID-01 (사전조건 위반 처리)
    def testRaisesTypeErrorForNonNumericInput(self):
        with self.assertRaises(TypeError):
            isWithinRange("5", 0, 10)
```

## 사용 가능한 테스트 설계 기법 (`@technique`)

기법의 정의와 전체 목록은 **`sw-quality-standards` 스킬의 `references/iso29119.md`(ISO/IEC/IEEE 29119 Part 4)를 단일 출처**로 사용합니다. 이 문서에서 기법을 다시 정의하지 않습니다 — 임의로 새 용어를 만들지 말고 반드시 그 문서의 명칭을 그대로 사용하세요.

유닛(함수/모듈) 수준에서 주로 쓰이는 기법과, 상세설계서의 어떤 요소를 근거로 삼는지는 다음과 같습니다:

| 기법 | 상세설계서의 근거 요소 (`@designRef`) |
|---|---|
| 동등분할 | 함수 계약의 입력 유효/무효 클래스 |
| 경계값 분석 | `algorithm-and-behavior-design.md`(상세설계)의 경계값 목록과 반드시 일치 |
| 결정 테이블 테스팅 | 상세설계의 정책 의사결정표(조건 조합) |
| 상태 전이 테스팅 | 상세설계의 상태전이 상세(상태/이벤트/가드) |
| 오류 추정 | 사전조건 위반, 잘못된 타입/형식 등 방어적 설계 항목(`defensive-programming-and-error-handling.md`) |

조합 테스트/페어와이즈/엣지 케이스 등 `iso29119.md`의 다른 기법이 유닛 수준에서 필요하다면 사용해도 되지만, 여러 입력 파라미터의 조합을 체계적으로 다뤄야 하는 요구사항 수준 테스트는 `sw-system-test` 스킬(아래 "다른 테스트 스킬과의 경계" 참조)의 몫입니다.

기법을 특정할 수 없는 임의 테스트(그냥 떠오르는 값)를 작성하지 마세요 — 반드시 근거를 선택하고, 상세설계서의 어떤 요소(경계값/조건조합/상태전이/오류경로)를 근거로 했는지 `@designRef`로 밝힙니다.

## 다른 테스트 스킬과의 경계

이 스킬(`tdd`)은 **A-SPICE SWE.4(소프트웨어 유닛검증)에 해당하는 유닛(함수/모듈) 수준** 테스트만 다룹니다. `coding` 에이전트가 상세설계서의 함수 계약을 근거로 구현과 동시에 작성하는 화이트박스 테스트입니다. 아래 인접 영역은 이 스킬의 범위가 아니며, 다른 스킬/에이전트가 담당합니다.

| 대상 | 담당 스킬/에이전트 | 이 스킬과의 차이 |
|---|---|---|
| 컴포넌트 간 인터페이스, 통합 순서 검증 (SWE.5) | `integration-testing` 스킬 / `integration-tester` 에이전트 | 테스트 베이시스가 함수 계약이 아니라 **아키텍처 설계서의 인터페이스 명세** |
| SW 요구사항 기반 자격시험 (SWE.6) | `sw-system-test` 스킬 / `sw-system-tester` 에이전트 | 테스트 베이시스가 상세설계가 아니라 **SW 요구사항 명세서**이며, 블랙박스 관점 |
| 산출물 관리 체계(CL2) 자체의 감사 | `aspice-auditor` 스킬 / `aspice-cl2-auditor` 에이전트 | 테스트를 직접 설계/실행하지 않고, 이미 작성된 산출물이 CL2 기준을 만족하는지만 점검 |

## 긍정/부정 케이스 분류 (`@case`)

- **긍정 (Positive)**: 사전조건을 만족하는 정상 입력에서 사양대로 동작하는지 검증
- **부정 (Negative)**: 사전조건 위반, 유효 범위 밖 입력, 예외 상황, 정의되지 않은 전이 등에서 방어적 동작(오류 반환/예외/거부)이 사양대로 동작하는지 검증

**규칙**: 핵심 함수/알고리즘마다 긍정 케이스만 있고 부정 케이스가 없다면 완료로 보고하지 마세요 — `SKILL.md`의 완료 조건("경계값과 오류 케이스가 커버되는가")과 `defensive-programming-and-error-handling.md`(상세설계 스킬)의 오류 처리 설계가 실제로 테스트되지 않은 것입니다.

## 결과 보고 시 집계

구현 결과 보고(`SKILL.md`의 "결과 보고 형식") 표에 `기법`, `케이스` 열을 채울 때는 이 문서의 용어를 그대로 사용하고, 대상 함수별로 긍정/부정 케이스가 모두 존재하는지 한눈에 확인할 수 있게 정리합니다.
