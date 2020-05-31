## 1.2 예시 프로그램을 본 소감
### 리팩토링이 필요한 이유
- `statement`함수를 수정해야 한다
- HTML태그를 삽입하거나 더 많은 타입의 연극에 대응해야 할 수 있다 -> 스펙 변경의 가능성이 있음
- `statement`함수, 혹은 복사본을 매번 수정해야 한다면 관리에 부담이 생긴다

## 1.3 리팩터링의 첫 단계
- 리팩터링 과정에서 테스트를 이용해 검증하는 것은 매우 중요하다. 시간단축 및 안정성 보장

## 1.4 statement() 함수 쪼개기
### `amountFor` 함수 추출하기
- `switch`문이 눈에 띈다
  - 한번의 공연에 대한 요금을 계산하는 로직
  - `6.1 함수 추출하기` 참고
  - `amountFor`함수로 추출했음
### `amountFor` 리팩토링 - 함수 내부에서 변수 이름을 명확하게 하기
  - 함수 내부에서 변수의 역할에 따라 이름을 짓는다
  - `thisAmount` -> `result`: 결과가 담긴다는 역할을 명확히 한다
  - `perf` -> `aPerformance`: js에서는 prefix로 타입이름을 명시해주면 좋은데, 타입이 명확하지 않다면 a/an을 붙인다.
    - 이 변수는 루프 안에서 매번 다른 값을 전달받는 변수다.
### `play` 변수 제거하기
  - `aPerformance`를 통해 얻을 수 있기 때문에 `amountFor`안에서 다시 계산하면 된다.
  - 긴 함수를 쪼갤때, 이런 변수들은 제거하는 편
  - 왜냐하면, 로컬 범위에 이름이 너무 늘어나면 작업이 복잡해지기 때문
    - `7.4 임시 변수를 질의 함수로 바꾸기`를 참고
  - `play` 변수를 제거: `6.4 변수 인라인하기`, `6.5 함수 선언 바꾸기`
  - 지금은 반복문에서 공연을 더 많이 조회한다. 하지만,
  - 지역 변수를 제거하면 유효범위를 신경 써야 할 대상이 줄어들어 추출 작업이 쉬워진다.
### `thisAmount` 제거하기
  - 포인트 적립 부분을 편하게 추출하기 위해 이 변수도 인라인한다
### 적립 포인트 계산 코드 추출하기
  - `volumeCreditsFor` 함수로 추출한다
  - 이 함수 내부의 변수도 마찬가지로 정리해준다
### `format`변수 제거하기
  - 임시 변수는 자신이 속한 루틴에서만 의미가 있다. -> 루틴을 간단하게 개선하려면 임시 변수를 제거 한다.
  - 현재는 `format`이 가장 제거하기 적절함
  - `format`변수를 제거하고 함수를 선언해 사용하도록 바꾼다.
  - 이 함수의 역할은 화폐 단위를 맞추기 위한 것이다. 근데 format이란 이름으로는 설명이 충분하지 않다.
  - 따라서 `6.5 함수 선언 바꾸기`를 이용한다.
### `volumeCredits`, `totalAmount` 변수 제거하기
  - 반복문안에 있기 때문에 먼저 `8.7 반복문 쪼개기`로 로직을 분리한다.
  - `8.6 문장 슬라이드하기`로 반복문 안에서 사용하는 변수의 선언부를 이동한다.
  - 이렇게 하는 이유는 `volumeCredits`값 갱신과 관련된 코드를 모아서 `7.4 임시 변수를 질의 함수로 바꾸기`를 수월하게 하도록 하기 위해서다.
  - 모아둔 코드를 `6.1 함수로 추출`하도록 한다.
  - 함수 추출이 끝나면, 앞에서처럼 `6.4 변수를 인라인`해서 제거 한다.

## 1.5 중간 점검: 난무하는 중첩 함수
- `statement` 함수는 일곱줄 뿐이며, 출력할 문장을 생성하는 역할에만 집중한다.
- 계산 로직들이 보조 함수로 분리되어서 전체 흐름을 이해하기 쉬워졌다.

## 1.6 계산 단계와 포맷팅 단계 분리하기
- 논리적인 요소를 파악하기 쉽도록 코드의 구조를 보강하는 작업이 끝났다!
- 이제 기능변경에 대응할 수 있도록 리팩토링 한다.
- HTML 버전의 결과물을 출력할 때, 기존에 분리해둔 계산 함수를 재사용할 수 있도록 하고싶다.
- `6.11 단계 쪼개기`를 사용해
  - 필요한 데이터를 처리하는 부분,
  - 처리한 결과를 HTML이나 텍스트로 표현하는 부분
  - 으로 분리한다.
- 단계를 쪼개기위해 `6.1 함수 추출하기`로 두번째 단계가 될 코드를 추출한다.
- 두 단계가 각자의 역할에 집중하도록 중간 데이터 구조를 생성한다.
  - statement로 계산 관련 코드를 모두 모은다.
  - statement는 계산 결과를 중간 데이터를 통해 renderPlainText로 전달한다.
  - renderPlainText는 invoice나 plays를 알 필요 없이 중간 데이터를 표현하는 역할만 집중한다.
- 단계가 분리된 이후, 각자의 파일로 나눠준다.

## 1.8 다형성을 활용해 계산 코드 재구성하기
- `amountFor`에는 연극 장르에 따라 계산방식이 달라지는 조건부 로직이 있다.
- `10.4 조건부 로직을 다형성으로 바꾸기`를 활용한다.
- 하지만 이 전에 상속 계층부터 정의해야 한다.
### 공연료 계산기 만들기
- 공연료 계산기 클래스를 만들었다.
- 계산기에서 필요한 연극 데이터를 `6.5 함수 선언 바꾸기`를 적용하여 전달해준다.
### 함수들을 계산기로 옮기기
- `8.1 함수 옮기기`를 활용해 계산하는 함수들을 계산기 클래스로 옮겨본다.
- `amountFor`를 계산기 클래스 getter로 복사해준다.
- `enrichPerformance`에서는 계산기의 getter를 이용하도록 한다.
- 테스트 덕분에 오타를 수정했다!
### 공연료 계산기를 다형성 버전으로 만들기
- `amount`와 `volumeCredits`은 연극 타입에 따라 다른 로직을 실행해야 한다.
- 계산기를 다형성을 이용하여 연극 타입에 따라서 다른 계산 결과를 얻을 수 있게 해본다.
  - 타입에 따라 알맞는 서브클래스를 사용하도록 한다.(`12.6 타입 코드를 서브클래스로 바꾸기`)
  - js에서는 생성자에서 서브클래스 인스턴스를 반환할 수 없다.
    - `11.8 생성자를 팩터리 함스로 바꾸기`를 적용한다.
    - 팩터리함수가 타입에 따라 다른 서브클래스 인스턴스를 반환한다.
- 각 서브클래스가 공연료를 다르게 계산할 수 있도록 `10.4 조건부 로직을 다형성으로 바꾸기`를 적용한다.
  - `amount`는 각 서브클래스에서 로직 구현을 강제하도록 `throw`에러를 던져줌.
  - `volumeCredits`는 공통 로직은 남겨두고, 서브클래스는 달라지는 부분만 오버라이드 하게 만들었다.

## 1.9 상태 점검: 다형성을 활용하여 데이터 생성하기
- 연극 장르별로 다르게 계산되는 코드들을 묶어 두었다.
  - 앞으로 대부분의 수정이 이 코드에서 이뤄질 것 같다는 가정이 있었음
  - 새 장르가 추가되면 새로운 서브클래스를 추가하면 된다.
  - 데이터 가공 및 렌더링하는 부분은 분리되어 있음