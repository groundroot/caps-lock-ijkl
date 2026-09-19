# Caps Lock + IJKL

Caps Lock을 짧게 눌렀다 떼면 F19를 보내고, 누른 채 I·J·K·L을 누르면 방향키로 사용하는 macOS용 Karabiner-Elements 규칙입니다.

## 키 동작

| 입력 | 출력 |
|---|---|
| Caps Lock 단독으로 짧게 눌렀다 떼기 | F19 |
| Caps Lock + I | ↑ |
| Caps Lock + J | ← |
| Caps Lock + K | ↓ |
| Caps Lock + L | → |
| Shift + Caps Lock + L | Shift + → |

Caps Lock을 누르지 않으면 I·J·K·L은 일반 문자키로 동작합니다. 방향키 조합을 사용하면 F19는 전송되지 않습니다.

## 설정 방법

Karabiner-Elements가 설치되어 있어야 합니다.

1. **Simple Modifications**에서 `caps_lock → f19`를 설정합니다. 이미 설정되어 있다면 그대로 유지합니다.
2. [caps lock +ijkl.json](caps%20lock%20%2Bijkl.json)을 열고 **Raw**에서 파일 내용 전체를 복사합니다.
3. **Complex Modifications → Add your own rule**을 엽니다.
4. 편집창의 예제 내용을 지우고 복사한 JSON을 붙여 넣은 뒤 **Save**를 누릅니다.
5. 기존 Caps Lock/F19 관련 복합 규칙이 있다면 테스트 중에는 비활성화합니다. 이 규칙은 충돌할 수 있는 관련 규칙보다 위에 배치합니다.

이 JSON은 **새 규칙 편집창에 넣는 단일 규칙**입니다. 전체 `karabiner.json`을 이 파일로 교체하지 마세요. 규칙 추가 절차는 [공식 설정 안내](https://karabiner-elements.pqrs.org/docs/manual/configuration/add-your-own-complex-modifications/)를 참고하세요.

## 동작 원리

Karabiner-Elements는 **Simple Modifications → Complex Modifications** 순서로 입력을 처리합니다. 따라서 `caps_lock → f19`로 변환한 입력을 받도록 이 규칙의 시작 키는 `f19`로 지정되어 있습니다. [입력 처리 순서](https://karabiner-elements.pqrs.org/docs/manual/misc/event-modification-chaining/)

F19를 누르면 `caps_f19_navigation` 변수를 `1`로 설정하고, 떼면 `0`으로 되돌립니다. 변수가 `1`인 동안 I·J·K·L을 방향키로 변환합니다. F19는 보조키가 아니므로 `mandatory: ["f19"]` 대신 이 변수로 눌림 상태를 구분합니다.

`optional: ["any"]` 설정으로 Shift·Command·Option·Control 등의 보조키도 방향키 출력에 유지됩니다. 실제 단축키 동작은 사용하는 앱에 따라 달라집니다. [보조키 처리 규칙](https://karabiner-elements.pqrs.org/docs/json/complex-modifications-manipulator-definition/from/modifiers/)

## F19 전송 시점

- F19는 Caps Lock을 누르는 순간이 아니라 **단독으로 짧게 눌렀다 뗄 때** 전송됩니다.
- 단독 입력 판정 제한은 `1000ms`입니다. 이 시간을 넘겨 누르고 있으면 F19가 전송되지 않습니다.
- 누르고 있는 동안 다른 키 입력, 마우스 버튼 입력 또는 스크롤이 발생하면 단독 입력 판정이 취소됩니다.
- 방향키 조합은 Caps Lock을 누른 직후 사용할 수 있으며, 1초를 기다릴 필요가 없습니다.
- F19를 누르고 있는 동안 실행하는 기능에는 적합하지 않습니다. 예를 들어 F19를 누르는 동안 녹음하는 방식과는 동작이 다릅니다.

필요하면 JSON의 `basic.to_if_alone_timeout_milliseconds` 값을 조정할 수 있습니다. 이 값은 단독 입력 판정 제한이며 방향키 활성화 대기 시간이 아닙니다. [to_if_alone 공식 문서](https://karabiner-elements.pqrs.org/docs/json/complex-modifications-manipulator-definition/to-if-alone/)

## 동작 확인

설정 후 텍스트 편집기에서 다음을 확인합니다.

- Caps Lock 없이 I·J·K·L을 누르면 문자가 입력됩니다.
- Caps Lock을 누른 채 I·J·K·L을 누르면 커서가 이동합니다.
- Shift + Caps Lock + L을 누르면 오른쪽으로 선택 영역이 확장됩니다.
- Caps Lock을 단독으로 짧게 눌렀다 떼면 F19에 연결한 단축키가 실행됩니다. F19에 동작을 지정하지 않았다면 화면상 반응이 없을 수 있습니다.

방향키가 동작하지 않으면 `caps_lock → f19` 설정과 기존 복합 규칙의 충돌 여부를 확인합니다. 복합 규칙은 위에서부터 검사하여 처음 일치하는 항목만 적용합니다. [복합 규칙 적용 우선순위](https://karabiner-elements.pqrs.org/docs/json/complex-modifications-manipulator-evaluation-priority/)
