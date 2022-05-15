# Router

> [vue router 공식문서](https://v3.router.vuejs.org/kr/installation.html)

## vue-router

- 라우팅 : 웹페이지 간의 이동 방법
- Vue.js의 공식 라우터
- 라우터는 컴포넌트와 매핑
- Vue를 이용한 SPA를 제작할 때 유용
- URL에 따라 컴포넌트를 연결하고 설정된 컴포넌트를 보여준다

CDN 작성

```html
<script src="https://unpkg.com/vue-router@3.5.3/dist/vue-router.js"></script>
```

## vue-router 이동 및 렌더링

index.html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vue.js</title>
    <style>
      .router-link-exact-active {
        color: red;
      }
    </style>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script src="https://unpkg.com/vue-router@3.5.3/dist/vue-router.js"></script>
  </head>
  <body>
    <div id="app">
      <h1>SSAFY - Router</h1>
      <p>
        <router-link to="/">HOME</router-link>
        <router-link to="/board">게시판</router-link>
        <router-link to="/qna">QnA</router-link>
        <router-link to="/gallery">갤러리</router-link>
      </p>
      <router-view></router-view>
      <div>만든이 : SSAFY</div>
    </div>
    <script type="module" src="app.js"></script>
  </body>
</html>
```

app.js

```jsx
// 라우트 컴포넌트
import Main from "./components/Main.js";
import Board from "./components/Board.js";
import QnA from "./components/QnA.js";
import Gallery from "./components/Gallery.js";

// 라우터 객체 생성
const router = new VueRouter({
  routes: [
    {
      path: "/",
      component: Main,
    },
    {
      path: "/board",
      component: Board,
    },
    {
      path: "/qna",
      component: QnA,
    },
    {
      path: "/gallery",
      component: Gallery,
    },
  ],
});

// Vue 인스턴트 라우터 주입
const app = new Vue({
  el: "#app",
  router,
});
```

components -> Board.js

```jsx
export default {
  template: "<div>자유 게시판</div>",
};
```

## $router, $route

- 라우터 설정

```jsx
const router = new VueRouter({
  routes: [
    {
      path: "/board/:no", // :뒤에 변수명
      component: BoardView,
    },
  ],
});
```

- 라우터 링크 :

```
<router-link to ="/board/30">30번 게시글</router-link>
```

- 전체 라우터 정보(router) : `this.$router`
- 현재 호출된 해당 라우터 정보(route) : `this.$route` `this.$route.params.no` `this.$route.path`

## vue-router 이동 및 렌더링

```jsx
export default {
  template: `<div>
    자유 게시판
    <ul>
      <li v-for="i in 5">
        <router-link :to="'/board/' + i">{{i}}번 게시글</router-link>
      </li>
    </ul>
  </div>`,
};
```

- to ="문자열"
- 변수 사용 위해서는 `:` bind 사용!

## 이름을 가지는 라우트

- 라우트는 연결하거나 탐색을 수행할 떄 이름이 있는 라우트를 사용 -> 별칭?
- Router Instance를 생성하는 동안 routes 옵션에 지정

```jsx
const router = new VueRouter({
  routes: [
    {
      path: "/",
      name: "main",
      component: Main,
    },
    {
      path: "/board",
      name: "board",
      component: Board,
    },
    {
      path: "/board/:no",
      name: "boardview",
      component: BoardView,
    },
    {
      path: "/qna",
      name: "qna",
      component: QnA,
    },
    {
      path: "/gallery",
      name: "gallery",
      component: Gallery,
    },
  ],
});
```

```jsx
export default {
  template: `<div>
    자유 게시판
    <ul>
      <li v-for="i in 5">
        <router-link :to="{name: 'boardview', params: {no: i}}">{{i}}번 게시글</router-link>
      </li>
    </ul>
  </div>`,
};
```

## 프로그래밍 방식 라우터

- `router-link`를 사용하여 선언적 네비게이션용 anchor 태그를 만드는 것 외에도 라우터의 instance method를 사용하여 프로그래밍으로 수행

```jsx
{
// 라우터 생성시 이름 설정
   path: '/board/:no',
   name: 'boardview',
   component: BoardView,
},
```

```jsx
export default {
  template: `<div>
    자유 게시판
    <ul>
      <li v-for="i in 5">
        <a :href="'#' + i" @click="$router.push({name: 'boardview', params: {no: i}})">{{i}}번 게시글</a>
      </li>
    </ul>
  </div>`,
};
```

## 중첩된 라우트

- 앱 UI는 일반적으로 여러 단계로 중첩된 컴포넌트 구조임
- URL의 세그먼트가 중첩 된 컴포넌트의 특정 구조와 일치하는 것을 활용
