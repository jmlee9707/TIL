# Vue.js 시작하기

[Vue 공식문서](https://kr.vuejs.org/)
[2021년 프론트엔트 프레임워크](https://2021.stateofjs.com/ko-KR/libraries/front-end-frameworks)

환경설정하기 - CDN 이용
[크롬 앱 스토어](https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd?hl=ko) vuejs-devtools 설치 후 [Vue 공식문서](https://kr.vuejs.org/)에서 script 개발버전 코드 추가

```html
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```

## MVVM Pattern

MVVM이란 Vue.js의 UI 화면 개발 방법 중 하나인 화면단 라이브러리로 화면을 `Model + view + ViewModel`로 구조화하여 개발하는 방식을 말한다.

> MVVM - 위키피디아
> 마크업 언어나 GUI 코드를 비즈니스 로직 or 백엔드 로직과 분리하여 개발하는 소프트웨어 디자인 패턴

![](https://velog.velcdn.com/images/jmlee9707/post/8f8ded1c-cc47-4eed-8058-d440617eee33/image.png)

- Model은 순수 자바스크립트 객체(plain JavaScript Objects), View는 웹페이지의 DOM(html), ViewModel은 Vue의 역할로 뷰와 모델의 중간영역이며 DOM Listener와 Data Binding을 제공하는 영역이다.
- 기존에는 자바스크립트로 view에 해당하는 DOM(html)에 접근하거나 수정하기 위해 jQuery와 같은 library를 이용했지만 Vue는 view와 \* Model을 `연결`하고 자동으로 바인딩하므로 `양방향 통신을 가능하게` 한다. -> **값이 바뀌더라도 자동으로 mapping**

- 중간 ViewModel : DOM Listeners(DOM의 값을 입력하거나 변경하면 data갱신을 Model(JS)에게 요청하고 받아옴)

## 선언적 렌더링

Vue.js 내부에서는 데이터와 DOM이 연결되어 모든 것이 반응형으로 이루어짐
Vue 앱은 단일 DOM 요소로 연결되어 DOM 요소를 완전히 제어

## 자바 스크립트 프레임워크 비교

> Vue.js / React.js / Angular.js

`Vue.js` => **프레임워크**
가장 최신에 등장한 프레임워크로 react와 angular의 장점을 가져와서 사용한다

- React : 단방향 데이터 흐름(컴포넌트간) ->
  - 부모 컴포넌트에서 자식 컴포넌트로 넘어갈 때 별 다른 설정 없이 사용 가능
  - Virtual Dom : 가상 돔 사용으로 PC 자원 덜 사용
- Angular.js : 양방향 데이터 바인딩 MVVM
  - 값이 바뀌더라도 자동으로 mapping

정해진 문법과 형태대로 사용하기에 개발자의 자유도가 떨어지며 프레임워크 형식이기에 기능을 부분적으로 사용할 수 없다
하지만 JavaScript의 문법을 몰라도 프레임워크 문법만 잘 알면 개발이 가능하다
컴포넌트 하나당 파일 하나로 구성된다

`React.js` => **라이브러리**
개발자의 자유도가 높으며 JavaScript를 잘 다루면 배우기가 쉽고 활용도가 높다
하지만 협업시에 코드 커뮤니케이션이 필요하며 파일 하나에 컴포넌트를 함수화 하여 여러개가 존재할 수 있다.

## SPA(Single Page Application)

> 한개의 페이지로 구성된 웹 서비스로 html 파일 크기가 커지면 유지 보수가 어렵기때문에 기존 페이지에 컴포넌트화된 서브페이지나 적절한 데이터만을 넘겨주어 client 상에 rendering이 이루어지는 CSR방식으로 동작한다

- 재사용 가능한 UI 설계(컴포넌트) -> vue.js 컴포넌트
- 화면 전환(컴포넌트 전환) -> vue router
- backend 통신 후 화면 반영 처리 : MVVM 디자인 패턴
