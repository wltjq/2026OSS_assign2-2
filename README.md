# OSS Assignment 04 - 회원 가입 신청서 Form

## Key Learning

이번 주 배운 핵심 내용 3가지

1. **HTML Form 요소의 종류와 용도** — `text`, `email`, `date`, `tel`, `color` 등 다양한 `input` 타입과 `select`, `textarea`, `datalist`, `fieldset`/`legend`로 Form을 만드는 방법을 배움.
2. **CSS로 Form의 사용성 개선** — 같은 마크업이라도 레이아웃, 여백, 포커스/호버 스타일을 입히면 사용자가 입력을 더욱 원활히 할 수 있게 도와주게 됨.
3. **HTML5 Validation + JavaScript Validation의 역할 분담** — `required`, `type="email"`, `minlength`, `checkValidity()` / `preventDefault()` / `focus()`를 조합한 것이 서로 보완한다는 것을 이해함.

## Form Elements

| 요소 | 용도 |
|---|---|
| `<input type="text">` | 이름, 출신 학교 등 자유 텍스트 입력 |
| `<input type="email">` | 이메일 입력 |
| `<input type="password">` | 비밀번호 입력 (마스킹 처리, `minlength`로 길이 제한) |
| `<input type="date">` | 생년월일을 날짜 선택기로 입력 |
| `<input type="tel">` | 전화번호 입력 |
| `<input type="radio">` | 성별 등 단일 선택 항목 |
| `<input type="checkbox">` | 관심 분야 등 다중 선택 항목 |
| `<input type="color">` | 선호 색상을 컬러 피커로 입력 |
| `<select>` / `<optgroup>` / `<option>` | 거주 지역을 그룹화된 목록에서 선택 |
| `<datalist>` | 출신 학교 입력 |
| `<textarea>` | 자기소개처럼 길이가 긴 텍스트 입력 |
| `<fieldset>` / `<legend>` | 관련 입력 항목을 그룹으로 묶고 제목을 부여 |
| `<label for="...">` | 각 입력 요소와 설명 텍스트를 연결해 접근성/클릭 영역 확보 |
| `<button type="submit">` / `<button type="reset">` | 폼 제출과 초기화 |

## HTML vs CSS

- `form1.html`은 스타일이 전혀 없는 순수 HTML 구조로, 브라우저 기본 스타일 그대로 입력창과 버튼이 위아래로 단순 나열된다.
- `form1_css.html`은 internal방식으로 `<style>`을 추가해 `form`을 카드 형태로 감싸고, `fieldset`에 테두리와 배경색을 입혔으며, `label` 너비를 고정해 정렬을 맞췄다.
- `input`/`select`/`textarea`에 `focus`, `hover` 스타일(테두리 색, 배경색 변화)을 추가해 사용자가 현재 어떤 입력창을 다루고 있는지 시각적으로 알 수 있게 했다.
- 즉 `form1.html`은 기능만 있는 상태이고, `form1_css.html`은 같은 기능에 레이아웃과 시각적 요소를 더한 상태다. 

## Validation & JS

**HTML Validation**

- 이름(`#name`), 이메일(`#email`), 비밀번호(`#password`) 3개 항목에 `required` 적용
- 이메일 입력에 `type="email"` 적용 → `@` 및 도메인 형식이 아니면 브라우저가 자동으로 막음
- 비밀번호 입력에 `minlength="6"` 적용 → 6자 미만 입력 시 유효하지 않은 값으로 처리

**JavaScript Validation**

```js
form.addEventListener("submit", function (event) {
  if (!name.checkValidity()) {
    event.preventDefault();
    alert("이름을 입력하세요.");
    name.focus();
    return;
  }
  if (!email.checkValidity()) {
    event.preventDefault();
    alert("유효한 이메일을 입력하세요.");
    email.focus();
    return;
  }
  if (!password.checkValidity()) {
    event.preventDefault();
    alert("비밀번호는 6자 이상 입력해야 합니다.");
    password.focus();
    return;
  }
  alert("등록이 완료되었습니다.");
});
```

- `form.addEventListener("submit", ...)`로 제출 이벤트를 가로챈다.
- `event.preventDefault()`로 실제 페이지 이동(기본 제출 동작)을 막는다.
- `checkValidity()`로 각 입력 요소가 required, type, minlength을 만족하는지 확인한다.
- 유효하지 않으면 해당 요소에 `focus()`를 주어 사용자가 바로 수정할 수 있게 하고, `return`으로 함수를 종료해 뒤 검사와 완료 알림이 실행되지 않게 한다.
- 이름 → 이메일 → 비밀번호 순으로 검사하며, 모든 조건을 통과하면 `alert("등록이 완료되었습니다.")`를 출력한다.

## Problem & Solution

- **문제**: `required`, `type="email"`만으로도 브라우저가 자동으로 제출을 막아주기 때문에, JavaScript 검증 코드가 실제로 실행되는지 확인하기 애매했다.

- **해결**: `checkValidity()`는 브라우저 기본 팝업이 뜨기 전에 값의 유효성을 먼저 코드로 검사할 수 있다는 것을 확인했고, 유효하지 않은 값과 유효한 값을 각각 입력해보며 `alert()` 메시지와 `focus()` 이동이 의도대로 동작하는지 직접 테스트했다.

## Reflection

- `required`, `type`, `minlength`만으로도 기본적인 유효성 검사는 브라우저가 알아서 처리해준다는 점이 새로웠다. 다만 커스텀 에러 메시지나 세밀한 제어(포커스 이동, 특정 알림 문구)를 하려면 결국 JavaScript로 `checkValidity()`를 직접 호출해야 한다는 것을 배웠다.
