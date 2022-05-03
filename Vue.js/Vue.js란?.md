# Vue.js란?
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
마크업 언어나 GUI 코드를 비즈니스 로직 or 백엔드 로직과 분리하여 개발하는 소프트웨어 디자인 패턴

![](https://velog.velcdn.com/images/jmlee9707/post/8f8ded1c-cc47-4eed-8058-d440617eee33/image.png)

* Model은 순수 자바스크립트 객체(plain JavaScript Objects), View는 웹페이지의 DOM(html), ViewModel은 Vue의 역할로 뷰와 모델의 중간영역이며 DOM Listener와 Data Binding을 제공하는 영역이다.
* 기존에는 자바스크립트로 view에 해당하는 DOM(html)에 접근하거나 수정하기 위해 jQuery와 같은 library를 이용했지만 Vue는 view와 * Model을 `연결`하고 자동으로 바인딩하므로 `양방향 통신을 가능하게` 한다. -> **값이 바뀌더라도 자동으로 mapping**

* 중간 ViewModel : DOM Listeners(DOM의 값을 입력하거나 변경하면 data갱신을 Model(JS)에게 요청하고 받아옴)

## 선언적 렌더링
Vue.js 내부에서는 데이터와 DOM이 연결되어 모든 것이 반응형으로 이루어짐
Vue 앱은 단일 DOM 요소로 연결되어 DOM 요소를 완전히 제어

