# 02 리팩터링 원칙

## 2.1 리팩터링 정의
마틴 파울러가 (일반적으로 사용되는 리팩터링이라는 용어를 좀 더 세부적으로) 정의하는 리팩터링은 다음과 같은 제약조건을 가진다.
1. 리팩터링 전 후의 사용자 관점에서의 동작은 동일해야 한다.
2. 리팩터링 전 후의 버그도 거의 동일하게 남아있어야 한다.
따라서 리팩터링 과정에서 코드는 항상 정상 작동하기 때문에
- 리팩터링 단계를 잘게 나눠 진행할 수 있다.
- 언제든지 리팩터링 과정의 어떤 단계에서든 멈춰도 동작에는 영향이 없다.
- 단계를 나눠서 디버깅 시간을 줄일 수 있다.
이를 따르면 효율적인 리팩터링이 가능하다.

## 2.2 두 개의 모자 - `기능 추가`와 `리팩터링`
- 작업 시간의 길이에 상관없이, 작업 영역을 구분(기능 추가와 리팩터링)해서 작업을 진행해야 한다.

## 2.3 리팩터링하는 이유 - 개발 속도를 빠르게
리팩터링을 하면 얻는 이득은 다음과 같다.
- 소프트웨어 설계가 좋아진다
- 소프트웨어를 이해하기 쉬워진다
- 버그를 쉽게 찾을 수 있다
- 프로그래밍 속도를 높일 수 있다
결론적으로 개발 속도를 향상시킨다. 일반적으로 리팩터링에 시간을 할애하면 개발 속도에 시간이 더 든다는 오해가 있다.
하지만 코드베이스의 규모가 커질수록, 새로운 기능을 기존 코드베이스의 구조를 고려하며 추가하는데 드는 시간이 점점 많이 든다.
따라서 지속적인 리팩터링은 결과적으로 개발 속도를 향상시키는 데 도움이 된다. (지구력 가설)

## 2.4 언제 리팩터링해야 할까?
계획된 리팩터링과 수시로 하는 리팩터링



