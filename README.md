# OSS Assignment 04 - 회원 가입 신청서 Form

## Key Learning

이번 주 배운 핵심 내용 3가지

1. **HTML Form 요소의 종류와 용도** — `text`, `email`, `date`, `tel`, `color` 등 다양한 `input` 타입과 `select`, `textarea`, `datalist`, `fieldset`/`legend`로 하나의 회원 가입 신청서를 구성하는 방법을 익혔다.
2. **CSS로 Form의 사용성 개선** — 같은 마크업이라도 레이아웃, 여백, 포커스/호버 스타일을 입히면 사용자가 입력 흐름을 훨씬 직관적으로 따라갈 수 있다는 것을 확인했다.
3. **HTML5 Validation + JavaScript Validation의 역할 분담** — `required`, `type="email"`, `minlength` 같은 선언적 검증과, `checkValidity()` / `preventDefault()` / `focus()`를 조합한 JavaScript 검증이 서로 보완 관계라는 것을 이해했다.

## Form Elements

| 요소 | 용도 |
|---|---|
| `<input type="text">` | 이름, 출신 학교 등 자유 텍스트 입력 |
| `<input type="email">` | 이메일 입력 및 형식(HTML5) 자동 검증 |
| `<input type="password">` | 비밀번호 입력 (마스킹 처리, `minlength`로 길이 제한) |
| `<input type="date">` | 생년월일을 날짜 선택기로 입력 |
| `<input type="tel">` | 전화번호 입력 |
| `<input type="radio">` | 성별 등 단일 선택 항목 |
| `<input type="checkbox">` | 관심 분야 등 다중 선택 항목 |
| `<input type="color">` | 선호 색상을 컬러 피커로 입력 |
| `<select>` / `<optgroup>` / `<option>` | 거주 지역을 그룹화된 목록에서 선택 |
| `<datalist>` | 출신 학교 입력 시 자동완성 후보 제공 |
| `<textarea>` | 자기소개처럼 길이가 긴 텍스트 입력 |
| `<fieldset>` / `<legend>` | 관련 입력 항목을 그룹으로 묶고 제목을 부여 |
| `<label for="...">` | 각 입력 요소와 설명 텍스트를 연결해 접근성/클릭 영역 확보 |
| `<button type="submit">` / `<button type="reset">` | 폼 제출과 초기화 |

## HTML vs CSS

- `form1.html`은 스타일이 전혀 없는 순수 HTML 구조로, 브라우저 기본 스타일 그대로 입력창과 버튼이 위아래로 단순 나열된다.
- `form1_css.html`은 동일한 마크업에 `<style>`을 추가해 `form`을 카드 형태로 감싸고, `fieldset`에 테두리와 배경색을 입혔으며, `label` 너비를 고정해 정렬을 맞췄다.
- `input`/`select`/`textarea`에 `focus`, `hover` 스타일(테두리 색, 배경색 변화)을 추가해 사용자가 현재 어떤 입력창을 다루고 있는지 시각적으로 알 수 있게 했다.
- 즉 `form1.html`은 "기능"만 있는 상태이고, `form1_css.html`은 같은 기능에 "레이아웃과 시각적 피드백"을 더한 상태다. 구조(HTML)와 표현(CSS)이 분리되어 있어야 유지보수가 쉽다는 점을 체감했다.

## Validation & JS

`form1_js.html`에서 `form1_css.html`을 기반으로 다음을 적용했다.

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
- `checkValidity()`로 각 입력 요소가 HTML5 제약 조건(required, type, minlength)을 만족하는지 확인한다.
- 유효하지 않으면 해당 요소에 `focus()`를 주어 사용자가 바로 수정할 수 있게 하고, `return`으로 함수를 종료해 뒤 검사와 완료 알림이 실행되지 않게 한다.
- 이름 → 이메일 → 비밀번호 순으로 검사하며, 모든 조건을 통과하면 `alert("등록이 완료되었습니다.")`를 출력한다.

## Problem & Solution

- **문제**: 과제 요구사항 보니까 "비밀번호 또는 문자열 입력에 minlength 적용"이라고 돼 있는데, 정작 form1_css.html에는 비밀번호 칸 자체가 없어서 어디에 적용해야 하나 좀 당황했다.
  **해결**: 그냥 비밀번호 입력 칸을 하나 새로 만들면 되겠다 싶어서 `type="password"`에 `minlength="6"`, `required`까지 같이 넣어줬다. 덕분에 required 3개(이름, 이메일, 비밀번호)랑 minlength 조건을 한 번에 해결할 수 있었다.
- **문제**: required랑 type="email"만 넣어도 브라우저가 알아서 막아주길래, 내가 만든 JS 코드가 진짜 실행되고 있는 건지 아니면 그냥 브라우저 기본 기능이 작동하는 건지 헷갈렸다. 처음엔 alert가 안 뜨길래 스크립트가 잘못된 줄 알고 콘솔도 찍어보고 그랬다.
  **해결**: 알고 보니 `checkValidity()`가 먼저 값이 맞는지 코드로 체크해주는 거였고, 일부러 이메일에 `@` 빼고 입력해보고 비밀번호도 짧게 넣어보면서 진짜로 alert랑 focus가 되는지 하나씩 눈으로 확인했다. 이런 식으로 일부러 틀린 값 넣어보는 게 디버깅에 도움이 많이 됐다.

## Reflection

- required, type, minlength 이런 것만 넣어도 브라우저가 기본적인 검증은 알아서 해준다는 게 신기했다. 근데 에러 메시지를 내 맘대로 바꾸거나 특정 칸으로 포커스를 옮기고 싶으면 결국 JS로 checkValidity()를 직접 불러줘야 한다는 걸 배웠다.
- event.preventDefault()를 안 써주면 checkValidity()가 false여도 브라우저가 자기 팝업을 띄우면서 제출을 막아버리는데, 이게 내가 만든 alert랑 순서상 어떻게 같이 동작하는 건지 정확히는 잘 모르겠다. 다음엔 noValidate 속성도 같이 써서 완전히 JS로만 검증하는 방식이랑 비교해보고 싶다.
- 궁금한 점: 지금은 이름, 이메일, 비밀번호를 하나하나 if문으로 따로 검사했는데, form.checkValidity()로 폼 전체를 한 번에 검사하는 방법도 있다고 들었다. 이렇게 하면 코드가 더 짧아질 것 같은데 어떤 요소가 문제인지 어떻게 알아내는지는 아직 잘 모르겠어서 다음에 찾아봐야겠다.
