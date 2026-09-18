# 네이밍 규칙과 Doxygen 주석 스타일

## 네이밍 규칙

CLAUDE.md에 따라 이 프로젝트는 Python 표준 관례(PEP 8의 snake_case)보다 **camelCase**를 우선합니다.

- 함수명: `calculateChecksum`, `validateInputRange` (동사로 시작하는 camelCase)
- 변수명: `retryCount`, `isValidInput` (camelCase, boolean은 `is`/`has` 접두어 권장)
- 테스트 메서드명: `testXxx` 형태의 camelCase (`unittest`가 `test`로 시작하는 메서드를 자동 탐색하므로 접두어 `test`는 고정, 뒤는 camelCase로 동작을 설명 — 예: `testReturnsFalseOutsideRange`)
- 클래스명은 CLAUDE.md에 별도 규정이 없으므로 Python 관례인 PascalCase를 유지합니다 (클래스명은 함수/변수명 규칙의 대상이 아님).
- 상수는 별도 규정이 없으므로 프로젝트 내 일관성을 우선하되, 사용자가 별도 지시하지 않는 한 기존 코드 스타일을 따릅니다.
- 정적분석 도구(`pylint`/`flake8`)의 기본 네이밍 규칙(snake_case 강제)과 충돌할 수 있으므로, 프로젝트 설정 파일(`.pylintrc`의 `function-naming-style`/`variable-naming-style` 등)에서 camelCase를 허용하도록 구성할 것을 사용자에게 제안합니다.

## Doxygen 주석 스타일

Python 코드에서 Doxygen이 인식할 수 있는 주석 형식을 사용합니다. `##` 블록과 `@brief`/`@param`/`@return`/`@throws` 태그를 사용하는 방식을 기본으로 합니다.

```python
## @brief 입력값이 유효 범위 내에 있는지 검사한다.
#  @param inputValue 검사할 입력값
#  @param minValue 허용 최소값 (포함)
#  @param maxValue 허용 최대값 (포함)
#  @return 유효 범위 내에 있으면 True, 아니면 False
#  @throws TypeError inputValue가 숫자형이 아닌 경우
def isWithinRange(inputValue, minValue, maxValue):
    if not isinstance(inputValue, (int, float)):
        raise TypeError("inputValue must be numeric")
    return minValue <= inputValue <= maxValue
```

- 모든 공개 함수/클래스에는 최소 `@brief` 한 줄을 작성합니다.
- 사전조건/사후조건/예외가 있는 함수(상세설계서의 함수 계약)는 `@param`, `@return`, `@throws`를 상세설계서 내용과 일치시켜 작성합니다 — 상세설계서와 다른 내용을 임의로 적지 않습니다.
- 모듈 상단에는 모듈 전체의 목적을 설명하는 `## @file` / `@brief` 블록을 작성합니다.
- **주석 비율 20% 이상**은 `quality-gates.md`의 측정 도구(라인 카운트) 결과로 확인합니다. 형식적으로 주석 줄 수만 채우기 위해 의미 없는 주석을 넣지 않습니다 — 각 주석은 실제로 함수 계약/알고리즘 근거를 설명해야 합니다.
- **테스트 함수의 Doxygen 주석**은 `@brief`뿐 아니라 `@technique`, `@case`(긍정/부정)를 함께 작성합니다. 상세 규칙과 예시는 `test-documentation-style.md`를 따릅니다.
