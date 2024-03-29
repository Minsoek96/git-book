# ServiceWorker

> 웹 브라우저가 백그라운드에서 실행하는 스크립트\
> 웹페이지와는 별도로 작동하고 프록시 서버 역할을 한다.

서비스 워커는 가로챈 리소스를 굉장히 세부적으로 캐싱할 수 있다.\
서비스워커는 브라우저와 독립적인 연결을 가진다.

## 사용 용도

* 인터넷이 끊어진 상황에서도 사용자가 웹 페이지에 접근할 수 있다.
* 푸시 알림을 생성
* 리소스 캐싱 웹 자원(HTML, CSS, javaScript등)을 캐시하여 빠르게 로드할 수 있게 하여, 특히 반복 방문 시 성능을 향상 시킨다.

## 프록시란? (대리로 통신)

인터넷 상의 여러 네트워크들에 접속할 때 중계 역할을 해주는 프로그램\
프록시는 요청을 가로챈 뒤 응답을 되돌려준다.

## MSW (Mock Service Woker)

서버 요청을 가로채서 모의 응답을 보내주는 라이브러리

* 외부의 의존성 없이 API요청을 테스트 할 수 있다.
* JavaScript코드로 작성되어 어떤 응답을 반환할지 명확하게 정의할 수 있다.
* 실제 브라우저 환경에서 네트워크 요청과 응답을 모방할 수 있어 다양한 시나리오를 테스트할 수 있다.
* 브라우저의 Service Woker를 활용화여 동작한다.

## polyfill(폴리필)

새로운 기능을 구형 브라우저에서 사용할 수 있도록 지원하는 코드나 플러그인

### Babel이랑 다를게 없는데?

Babel은 JavaScript 코드 변환을 위한 도구

> 최선 JS(ES6,ES7등) 코드를 ES5 버전으로 변환해\
> 구형 브라우저에서도 해당 코드가 실행 가능하게 하는 목적

Babel은 새로운 JS문법 (화살표 함수 , 클래스 등)을 구형 브라우저에서 실행될 수 있는 구문 으로 변환

Polifill은 구형 브라우저에서 기본적으로 지원하지 않는 기능!!을 제공하기 위해 사용\
`Promise`, `fetch`, `Array.from`과 같은 최신 JavaScript API등

Babel은 문법 변환에 집중\
Polyfill은 기능 구현에 집중
